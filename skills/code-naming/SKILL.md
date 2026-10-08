---
name: 代码文件命名
description: 在 Java 后端或 TS + React 前端项目里新建、命名、改名或移动代码文件和目录时使用；也用于审查文件命名是否合规。
---
# 代码文件命名（Java 后端 + TS/React 前端）

管两件事：新文件怎么起名，旧文件怎么安全改名。思路参考 erichugy file-naming、Genyus swarm、mondaycom/vibe、rilldata/rill 的命名规则，以及 python-rope-refactor 的安全改名流程；本 skill 自包含。

## 优先级（冲突时从上往下）

1. 项目已有约定：先读 `AGENTS.md`、`CLAUDE.md`、`.cursor/rules/`、`README`、lint 配置（Checkstyle、ESLint）
2. 所在目录的主流写法：同目录已有 10 个 `PascalCase.tsx`，新文件也用 `PascalCase.tsx`
3. 框架保留名：不改
4. 本 skill 的默认规则

新文件立即遵守。旧文件不批量改，只在本来就要修改它时顺手迁移，并且要先问用户。

## 通用规则

- 目录名说明领域，文件名说明用途。不重复目录已有的上下文：`order/OrderService.java` 可以（Java 要求类名完整），前端 `features/order/order-api.ts` 写成 `features/order/api.ts`
- 一个文件一个主要导出，文件名与它对应
- 禁用笼统名：`utils`、`helpers`、`common`、`misc`、`shared`、`stuff`、`temp`、`test2`
- 禁用版本和状态名：`*-v2`、`*New`、`*Old`、`old-*`、`index2`、`Copy of *`、`data1`
- 不用含义不清的缩写（`usrMgr`）；行业通用缩写可以（`id`、`url`、`dto`、`api`）
- 文件名里不出现空格、中文、`#`、`&` 等特殊字符

---

## Java 后端

### 硬规则（编译器或 Java 惯例决定）

- 文件名 = 公有顶层类名，PascalCase：`OrderService.java` 里是 `public class OrderService`
- 包名全小写，不用下划线和大写：`com.example.order.service`
- 一个 `.java` 文件只放一个公有顶层类
- 包路径与目录一致：`com/example/order/service/OrderService.java`

### 后缀约定（Spring Boot 常见写法，以项目已有写法为准）

| 角色 | 命名 | 示例 |
|---|---|---|
| 控制器 | `XxxController` | `OrderController.java` |
| 服务接口 / 实现 | `XxxService` / `XxxServiceImpl` | 项目没有接口就只用 `XxxService` |
| 数据访问 | `XxxRepository`（JPA）或 `XxxMapper`（MyBatis） | `OrderMapper.java` |
| 实体 | `Xxx` 或 `XxxEntity`，与项目一致 | `Order.java` |
| 传输对象 | `XxxDTO`、`XxxRequest`、`XxxResponse`、`XxxVO` | `CreateOrderRequest.java` |
| 配置 | `XxxConfig` 或 `XxxConfiguration` | `RedisConfig.java` |
| 异常 | `XxxException` | `OrderNotFoundException.java` |
| 枚举 | 名词，PascalCase | `OrderStatus.java` |
| 常量 | `XxxConstants` | `OrderConstants.java` |
| 工具类 | 按用途命名，不要 `CommonUtils` | `GeoDistanceCalculator.java` |
| 单元测试 | `XxxTest` | `OrderServiceTest.java` |
| 集成测试 | `XxxIT` 或 `XxxIntegrationTest` | `OrderControllerIT.java` |

缩写词当普通单词写，与项目统一：`HttpClient`、`DtoMapper`（或项目已用的 `DTOMapper`），不要混用。

### 目录结构

- 按功能分包（`order/`、`radar/`）还是按层分包（`controller/`、`service/`），沿用项目现有方式，不在同一项目里混用
- 测试与源码包路径一致：`src/test/java/com/example/order/OrderServiceTest.java`

### 资源文件

- 配置：`application.yml`、`application-{profile}.yml`（`application-dev.yml`）
- MyBatis XML 与 Mapper 接口同名：`OrderMapper.xml`，`namespace` 写完整类名
- 数据库迁移脚本按工具约定：Flyway 用 `V{版本}__{描述}.sql`（两个下划线），如 `V3__add_order_index.sql`

---

## TS + React 前端

### 先定两个选择，全项目统一

| 选项 | A（社区常见） | B（全 kebab） |
|---|---|---|
| 组件文件 | `UserCard.tsx` | `user-card.tsx` |
| hook 文件 | `useAuth.ts` | `use-auth.ts` |

项目已有写法就沿用；新项目没有约定时用 A。不在同一项目里混用两种。

### 默认规则

| 类别 | 命名 | 示例 |
|---|---|---|
| 目录 | kebab-case | `features/track-replay/` |
| 普通模块、工具 | kebab-case | `format-time.ts` |
| 组件 | 见上表 | `TrackTimeline.tsx` |
| hook | 以 `use` 开头 | `useReplayClock.ts` |
| 类型 | `.types.ts` | `track.types.ts` |
| 接口请求 | `.api.ts` 或放 `api/` 目录 | `track.api.ts` |
| 状态管理 | `.store.ts` | `replay.store.ts` |
| 校验 schema | `.schema.ts` | `query.schema.ts` |
| 常量 | `.constants.ts` | `map.constants.ts` |
| 样式 | 与组件同名 | `TrackTimeline.module.css` |
| 测试 | 与源文件同名加 `.test` | `TrackTimeline.test.tsx` |
| Storybook | `.stories.tsx` | `TrackTimeline.stories.tsx` |
| 配置 | 工具约定名 | `vite.config.ts`、`tsconfig.json` |

- 自己写的类型放 `.types.ts`，不放 `.d.ts`；`.d.ts` 只用于给第三方库或全局补声明
- 接口名不加 `I` 前缀
- 框架保留名不改：`main.tsx`、`App.tsx`（若项目已用）、`vite-env.d.ts`，以及路由框架约定的 `page.tsx`、`layout.tsx`

### 目录结构

- 按功能分目录，相关文件就近放：组件、样式、测试、类型在同一个功能目录里
- 测试和源文件放一起，或放同级 `__tests__/`，沿用项目现有方式；不要全部堆到根目录
- `index.ts` 只用来汇总导出，只放在功能边界；不在里面写实现，不层层 `export *`
- 全局 `components/`、`hooks/`、`utils/`、`types/` 只放跨功能共用的东西

---

## 安全改名流程

改名或移动文件时按顺序做，不跳步：

1. **查引用**：用 `rg` 搜旧名，列出所有命中，先给用户确认范围
   - Java：`import`、包声明、Spring XML、MyBatis XML 的 `namespace` 和 `resultType`、`application*.yml`、反射或字符串里的类名、测试
   - 前端：`import`、动态 `import()`、`React.lazy`、`index.ts` 导出、`tsconfig` 路径别名、测试里的 `vi.mock` 或 `jest.mock`、Storybook、文档
2. **改名**：用 `git mv` 保留历史。只改大小写时（Windows、macOS 默认不区分大小写）分两步：`git mv Foo.tsx Foo.tmp.tsx`，再 `git mv Foo.tmp.tsx foo.tsx`
3. **改引用**：能用工具就用工具。Java 优先 IDE 的重构改名（同时改包声明和类名）；TS 优先编辑器的重命名，它会同步改 import
4. **Java 额外检查**：文件名改了，类名必须跟着改；移动了包，包声明必须跟着改
5. **确认**：`rg` 旧名零命中（历史日志和 CHANGELOG 除外）
6. **验证**
   - 后端：`./mvnw compile`，再 `./mvnw test`（Windows 用 `.\mvnw.cmd`）
   - 前端：`npx tsc --noEmit`、`npm run lint`、`npm test`、`npm run build`
7. 前端文件里导出的名字不随文件名改动，除非用户要求

## 可选：用 lint 强制

- Java：Checkstyle 的 `TypeName`、`PackageName`、`OuterTypeFilename` 规则
- 前端：ESLint 的 `eslint-plugin-check-file`（文件和目录命名）或 `unicorn/filename-case`

只在用户同意时添加 lint 配置，不擅自改项目工具链。

## 审查清单

- [ ] 与项目已有约定和同目录写法一致
- [ ] Java 文件名与公有类名一致，包名全小写且与目录一致
- [ ] 前端组件和 hook 的大小写方案全项目统一
- [ ] 没有禁用名（`utils`、`*-v2`、`temp` 等）
- [ ] 测试文件名与源文件对应
- [ ] 改名后旧名零命中，编译、lint、测试通过
