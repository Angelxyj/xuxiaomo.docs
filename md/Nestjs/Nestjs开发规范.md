# Nestjs 开发规范

## 一、请求生命周期

1. 传入请求
2. 全局中间件  → 模块中间件
3. 全局守卫  → 控制器守卫  → 路由守卫
4. 全局拦截器  → 控制器拦截器  → 路由拦截器
5. 全局管道  → 控制器管道  → 路由管道  → 路由参数管道
6. 控制器逻辑  → Service → Dao
7. 路由拦截器  → 控制器拦截器  → 全局拦截器
8. 路由异常过滤器  → 控制器异常过滤器  → 全局异常过滤器
9. 返回响应

## 二、Provider 使用场景

### Middleware 中间件

- 认证和授权 ： 验证用户身份、角色、权限，针对特定路由进行访问控制；
- 日志记录 ：记录请求时间、IP 地址、用户信息等公共基础日志；
- 数据转换 ：比如 cookie 结构化数据转换等；
- 函数式中间件适合逻辑简单，无成员、无依赖项。

### Guard 守卫

- 鉴权 ：守卫可以通过执行上下文环境获取元信息从而针对路由进行更精细的控制；

#### auth.guard.ts

```ts
@Injectable()
export class AuthGuard implements CanActivate {
  private readonly logger: AppLogger = new AppLogger(AuthGuard.name, {
    timestamp: false,
  });

  constructor(private readonly reflector: Reflector) {}

  canActivate(
    context: ExecutionContext
  ): boolean | Promise<boolean> | Observable<boolean> {
    const meta = this.reflector.get<AuthMeta>("meta", context.getHandler());
    const raw = context.switchToHttp().getRequest().raw as IncomingMessage & {
      user: User;
      authorization: Authorization;
      originalUrl: string;
    };
    const { user, authorization, originalUrl: path } = raw;
    if (meta.uri) {
      if (!authorization || !authorization.hasURIAuth(meta.uri)) {
        throw new ForbiddenException({
          code: RESPONSE.CODES.FORBIDDEN,
          message: "You do not have access",
        });
      }
    }

    return true;
  }
}
```

### Interceptor 拦截器

- 路由处理时间 ：计算路由消耗时间；
- 响应数据统一处理 ：针对响应数据转换为相同数据结构；
- 异常映射 ：错误异常 CODE 分类映射；
- 缓存覆盖 ：命中缓存后直接返回数据，跳过控制器逻辑；

#### logging.interceptor.ts

```ts
@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log("Before...");

    const now = Date.now();
    return next
      .handle()
      .pipe(tap(() => console.log(`After... ${Date.now() - now}ms`)));
  }
}
```

### Pipe 管道

- 转换 ：将输入数据转换为需要格式，可以结合 class-transformer 进行使用；
- 验证 ：校验输入数据是否有效，可以结合 class-validator 进行使用；

#### controller.ts

```ts
@Post()
async create(
@Body(new ValidationPipe()) createCatDto: CreateCatDto,
) {
this.catsService.create(createCatDto);
}
```

#### validate.pipe.ts

```ts
import {
  PipeTransform,
  Injectable,
  ArgumentMetadata,
  BadRequestException,
} from "@nestjs/common";
import { validate } from "class-validator";
import { plainToInstance } from "class-transformer";

@Injectable()
export class ValidationPipe implements PipeTransform<any> {
  async transform(value: any, { metatype }: ArgumentMetadata) {
    if (!metatype || !this.toValidate(metatype)) {
      return value;
    }
    const object = plainToInstance(metatype, value);
    const errors = await validate(object);
    if (errors.length > 0) {
      throw new BadRequestException("Validation failed");
    }
    return value;
  }

  private toValidate(metatype: Function): boolean {
    const types: Function[] = [String, Boolean, Number, Array, Object];
    return !types.includes(metatype);
  }
}
```

#### create.cat.input.ts

```ts
import { IsString, IsInt } from "class-validator";

export class CreateCatDto {
  @IsString()
  name: string;

  @IsInt()
  age: number;

  @IsString()
  breed: string;
}
```

- ExceptionFilter 异常过滤器
  异常捕获 ：负责程序中未处理异常捕获，返回友好响应；

#### http-ception.filter.ts

```ts
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  private readonly logger: AppLogger = new AppLogger(HttpExceptionFilter.name, {
    timestamp: false,
  });

  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<FastifyReply>();
    const request = ctx.getRequest<FastifyRequest>();
    const status = exception.getStatus();
    const exceptionMessage: any = exception.message;

    const responseJSON: ErrorJSONResponse = {
      code: exceptionMessage.code || RESPONSE.CODES.ERROR,
      msg: exceptionMessage.message || RESPONSE.MESSAGES.ERROR,
      data: null,
      ts: new Date().toISOString(),
      path: request.url,
    };

    this.logger.error(`[exception: ${exception}]`);

    response.status(status).send(responseJSON);
  }
}
```

##三、目录结构

```
project-root // 项目根目录
├── node_modules
├── scripts
├── src
│ ├── core // 核心目录，存放中间件、守卫等全局 provider
│ │ ├── decorators // 全局自定义装饰器
│ │ ├── middlewares
│ │ ├── guards
│ │ ├── interceptors
│ │ ├── pipes
│ │ ├── filters
│ │ └── base // 存放 baseController，baseService 等可供被继承的基础 provider，包含公共方法
│ ├── utils // 工具函数
│ ├── config // 全局配置
│ ├── interfaces // 全局 Type、Interface 定义
│ ├── promote  // 业务模块
│ │ ├── config  // 业务模块内部配置
│ │ ├── interfaces  // 业务模块内部 Type 和 Interface 定义
│ │ ├── entities // 实体类，用于接口返回，其中使用 class-transformer 进行数据的转换处理，应以 entity 进行命名
│ │ │ ├── x.entity.ts
│ │ │ └── y.entity.ts
│ │ ├── dto // 数据传输对象，用于接收接口请求参数，其中使用 class-validator 进行数据合法性验证，应以 input 进行命名
│ │ │ ├── promote.aa.input.ts
│ │ │ └── promote.bb.input.ts
│ │ ├── dao // 请求示例，针对于不同第三方服务，应当创建不同 dao 对象，其中请求头设置、返回数据检查等差异，应该封装在内部
│ │ │ ├── promote.x.dao.ts
│ │ │ └── promote.y.dao.ts
│ │ ├── promote.module.ts // 模块入口文件
│ │ ├── promote.aa.controller.ts // 控制器，定义路由，可以根据当前模块业务进行拆分
│ │ ├── promote.aa.service.ts // 负责业务逻辑串联，数据转换，接口合并
│ │ ├── promote.bb.controller.ts
│ │ └── promote.bb.service.ts
│ ├── cache  // 缓存模块，其他业务模块或者工具模块应当在此层级进行创建
│ ├── graphql
│ ├── app.module.ts  // app 根模块
│ └── main.ts  // 主文件，负责 app 示例创建，绑定全局插件、中间件等
├── package.json
└── README.md
```

## 四、数据转换

### detail.entity.ts

```ts
export class QuickTemplateDetailEntity {
@Expose() quickTaskName: string;
@Expose() keepFrontData: string;
@Expose() episodeInfo: EpisodeInfo;
@Expose() advertiserInfoList: Array<AdvertiserInfo>;
@Transform((params) => {
const obj: QuickTemplatePreviewBackendDto = params.obj;
return obj.adLv1Param.adLv1Name;
})
@Transform((params) => {
const obj: QuickTemplatePreviewBackendDto = params.obj;
const value = obj.adLv1Param;
return plainToClass(
AdLv1Param,
{
...value,
downloadMode: value.downloadMode === DOWNLOAD_MODE.APP_STORE_DELIVERY,
productSetting:
value.relatedProduct.productSetting === PRODUCT_SETTING.SINGLE,
productPlatformId: value.relatedProduct.productPlatformId,
productId: value.relatedProduct.productId,
deliverySetting: {
...value.deliverySetting,
budgetOptimizeSwitch:
value.deliverySetting.budgetOptimizeSwitch === SWITCH_STATUS.ON,
},
},
{
excludeExtraneousValues: true,
}
);
})
// ...
}
```

### service.ts

```ts
// ...
async queryQuickTemplateDetail(
channelCode: string,
id: number
): Promise<JSONResponse<QuickTemplateDetailEntity>> {
this.logger.log(
`[PromoteService] [queryQuickTemplateDetail] [params: ${id}]`
);

    try {
      const result = await this.adsDao.queryQuickTemplateDetail(channelCode, {
        templateId: id,
      });

          // ...

      return {
        ...result,
        data: plainToClass(QuickTemplateDetailEntity, result.data.content, {
          excludeExtraneousValues: true,
        }),
      };
    } catch (err) {
      return {
        code: RESPONSE.CODES.ERROR,
        msg: err.message,
        data: null,
      };
    }

}
```
