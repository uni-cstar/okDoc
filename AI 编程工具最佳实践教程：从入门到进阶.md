# AI 编程工具最佳实践教程：从入门到进阶

> 以 Cursor 为例，原则通用于 GitHub Copilot、ChatGPT 等工具（Java 开发者版）

---

## 1. 核心理念与工作流：AI 时代的"双人舞"

AI 编程工具（如 Cursor, GitHub Copilot, ChatGPT for Code 等）的出现，标志着软件开发进入了一个新的协作时代。开发者不再是孤军奋战的"独奏者"，而是与 AI 共同演奏的"双人舞"主角。

### 1.1 AI 编程工具的定位：副驾驶（Copilot）

AI 工具的本质是**代码补全、知识检索和模式识别的强大引擎**，它是一个高效的**副驾驶（Copilot）**，而非可以完全替代人类的"主驾驶"。

| 角色 | 职责 | 核心能力 |
| :--- | :--- | :--- |
| **开发者（主驾驶）** | 设定目标、架构设计、业务逻辑、**最终审查与测试** | 批判性思维、领域知识、系统设计、质量保证 |
| **AI 工具（副驾驶）** | 快速生成样板代码、知识检索、代码解释、重构建议 | 模式识别、代码生成、多语言翻译、上下文理解 |

**核心理念：** 将 AI 视为一个**极速且永不疲倦的初级工程师**。它能迅速完成 80% 的繁琐工作，但最后的 20%——**业务逻辑的准确性、安全漏洞的排查、性能的优化**——始终需要人类开发者来把关。

### 1.2 最佳协作工作流：构思-生成-审查-迭代

最高效的 AI 编程工作流是**小步快跑、持续反馈**的迭代过程，而非一次性生成整个项目。

1.  **构思与分解（Developer）：** 开发者首先在脑中或文档中清晰定义要解决的问题，并将复杂任务分解为 AI 可以处理的原子任务（例如：实现一个 Service 方法、定义一个 DTO 接口、编写一个单元测试用例）。
2.  **提示与生成（AI）：** 开发者提供**精准的提示词**和**必要的上下文**，让 AI 生成代码草稿。
3.  **审查与测试（Developer）：** **这是最关键的一步。** 开发者必须仔细审查 AI 生成的代码，检查其逻辑、风格、安全性和性能，并立即运行测试。
4.  **迭代与精炼（Developer & AI）：** 如果代码不符合要求，开发者不应直接修改，而是通过**追加提示词**（例如："请将这个方法改为异步模式，并使用 `CompletableFuture`"）来引导 AI 进行修改，直到满意为止。

---

## 2. 提示词工程精髓：如何与 AI 高效对话

提示词工程是最大化 AI 编程工具价值的**核心技能**。一个好的提示词能让 AI 的输出质量从 50% 提升到 95%。

### 2.1 清晰定义任务上下文（Context Is King）

AI 无法"看"到你的整个项目，你必须主动提供它所需的一切信息。

| 元素 | 作用 | 最佳实践 |
| :--- | :--- | :--- |
| **技术栈** | 确定代码语言和框架 | 明确指定：`使用 Java 17`、`基于 Spring Boot 3.x`、`使用 MyBatis-Plus`。 |
| **相关代码片段** | 提供依赖关系和接口定义 | 在 Cursor 中，使用 **选中代码** 或 **`@` 符号** 引用文件或代码块。 |
| **目标文件** | 确定代码的存放位置 | 在 Cursor 的 `Cmd+K` 或 `Cmd+L` 模式中，确保光标位于正确的文件或位置。 |
| **问题描述** | 告诉 AI 你想做什么 | 使用动词开头，如：`实现...`、`重构...`、`修复...`。 |

**Cursor 特色操作：**

*   **选中代码：** 在编辑器中选中一段代码，然后按下 `Cmd+K`（行内编辑）或 `Cmd+L`（聊天），AI 会将选中内容作为最高优先级的上下文。
*   **`@` 引用：** 在聊天框中输入 `@` 符号，可以引用项目中的文件、符号（方法、类）或终端输出，将它们作为上下文喂给 AI。

### 2.2 任务分解：化繁为简

AI 在处理**具体、单一、目标明确**的任务时表现最佳。避免让 AI 一次性完成一个宏大的、多步骤的任务。

| 任务类型 | 错误示例（低效） | 正确示例（高效） |
| :--- | :--- | :--- |
| **功能实现** | "请帮我写一个完整的用户注册和登录系统。" | 1. "请实现 `User` 实体类，包含 `username`, `email`, `passwordHash` 字段，使用 JPA 注解。" 2. "请为 `UserService` 编写一个 `hashPassword` 方法，使用 BCrypt。" 3. "请编写注册 Controller，使用 Spring Security 和 JWT 进行认证。" |
| **重构** | "请优化这个类中的所有代码。" | 1. "请将这个 100 行的方法拆分为 3 个职责单一的私有方法。" 2. "请将 `UserService` 中的所有原始类型包装为 Optional。" |

### 2.3 明确约束与要求：精准控制输出

通过添加约束条件，可以显著提高 AI 输出的可用性。

| 约束类型 | 示例提示词 | 作用 |
| :--- | :--- | :--- |
| **风格/规范** | `请严格遵循阿里巴巴 Java 开发手册规范。` `使用 Java 17 特性（如 record、sealed class）。` `不要使用 @Autowired 字段注入，使用构造器注入。` | 确保代码风格统一，符合项目规范。 |
| **性能/库** | `请使用 Guava 的 RateLimiter 实现限流。` `请确保查询使用了索引，复杂度为 O(1)。` `使用 JPA Criteria API，不要使用原生 SQL。` | 限制 AI 使用特定的库或算法，满足性能要求。 |
| **输出格式** | `只输出代码，不要解释。` `请用表格总结重构前后的性能差异。` `请在代码块中输出，并标注语言为 java。` | 控制 AI 的输出形式，便于复制粘贴或阅读。 |

### 2.4 迭代与精炼：从草稿到成品

当 AI 的初始输出不完美时，不要放弃，而是通过追加提示词进行精炼。

| 迭代目标 | 常用精炼话术 |
| :--- | :--- |
| **修改逻辑** | `不对，在处理空值时应该返回 Optional.empty()，而不是抛出异常。` |
| **改变风格** | `请将这段代码中的所有注释改为 Javadoc 格式。` |
| **添加功能** | `很好，现在请为这个方法添加一个缓存机制，使用 Spring Cache 的 @Cacheable 注解。` |
| **简化/优化** | `这段代码太冗长了，请用 Stream API 和 Lambda 表达式简化它。` |

---

## 3. Cursor 核心功能实战指南

以 Cursor 为例，其核心功能对应了开发者的不同需求场景。掌握这些工具的最佳使用场景是提高效率的关键。

| 功能名称 | 快捷键 | 最佳使用场景 | 提示词最佳实践 |
| :--- | :--- | :--- | :--- |
| **聊天交互** | `Cmd+L` | 架构讨论、跨文件问题、复杂逻辑解释、项目级设计建议。 | 引用多个文件（`@UserService.java @UserRepository.java`），提供宏观上下文。 |
| **行内编辑** | `Cmd+K` | **局部代码生成、快速修改、方法实现**、添加注释。 | **选中代码块**，提示词简短明确，如：`实现这个方法`、`修复这个 bug`。 |
| **代码重构** | `Cmd+L` (选中代码后) | 安全地重命名变量、提取方法、简化逻辑、应用设计模式。 | 明确指定重构目标和范围，如：`请将这段逻辑提取为一个名为 'processData' 的私有方法。` |
| **代码理解** | `Cmd+Shift+L` (或选中代码后 `Cmd+L` 提问) | 快速理解陌生代码、复杂算法、第三方库的用法。 | 提问具体，如：`这个方法的核心逻辑是什么？` `它在什么情况下会抛出异常？` |

### 3.1 代码生成 (`Cmd+K`) 的艺术

`Cmd+K` 是最常用的功能，用于在当前光标位置或选中区域进行快速的代码生成或修改。

**最佳实践：**

1.  **先写方法签名/注释：** 在调用 `Cmd+K` 之前，先写下你想要的方法名、参数列表，甚至是一个简短的 Javadoc。这为 AI 提供了极强的约束。
2.  **选中上下文：** 如果你的方法依赖于外部字段或其他方法，请选中它们，或在提示词中明确提及。

**示例（Java）：** 目标是实现一个文件读取方法。

```java
// 1. 预先写好方法签名和注释
/**
 * 读取指定路径的 JSON 文件内容。
 * 如果文件不存在，返回一个空的 Map。
 *
 * @param filePath 文件路径
 * @return JSON 内容转换后的 Map
 */
public Map<String, Object> readJsonFile(String filePath) {
    // 2. 在这里按下 Cmd+K，并输入提示词
    // 提示词: 实现这个方法，使用 Jackson ObjectMapper，并用 try-catch 捕获 IOException
}
```

### 3.2 代码重构 (`Cmd+L` 或 `Cmd+K`) 的安全网

重构是 AI 编程工具最强大的功能之一，但也是风险最高的。

**重构前后的关键检查点：**

1.  **运行测试：** 确保重构前所有单元测试通过（`mvn test` 或 `gradle test`）。
2.  **明确范围：** 选中要重构的代码块，或在提示词中明确指定文件/方法名。
3.  **审查差异：** AI 完成重构后，**必须**使用版本控制工具（如 Git Diff）仔细检查所有修改，确保逻辑没有被意外改变。
4.  **再次运行测试：** 确保重构后所有单元测试仍然通过。

**示例（Java）：** 简化一个复杂的条件判断。

```java
// 原始代码 (选中这段代码)
public boolean checkUserAccess(User user, String resource) {
    if (user.isAdmin()) {
        return true;
    } else if (user.getStatus() == UserStatus.ACTIVE 
               && user.getPermissions().contains(resource)) {
        return true;
    } else if (user.getGroups().contains("premium") 
               && "premium_content".equals(resource)) {
        return true;
    } else {
        return false;
    }
}

// 提示词 (Cmd+L 或 Cmd+K):
// 请使用更简洁的布尔逻辑表达式重构这个方法，避免多余的 if/else 结构，使用 Java 17 特性。
```

**AI 可能生成的重构结果：**

```java
public boolean checkUserAccess(User user, String resource) {
    return user.isAdmin()
        || (user.getStatus() == UserStatus.ACTIVE && user.getPermissions().contains(resource))
        || (user.getGroups().contains("premium") && "premium_content".equals(resource));
}
```

---

## 4. 实战案例库：多场景提示词示例

以下是针对不同 Java 开发任务的优秀提示词模板和案例。

### 4.1 场景 A：实现特定功能

| 任务 | 提示词模板 | 关键约束 |
| :--- | :--- | :--- |
| **数据处理** | `实现一个方法，接收一个 List<Map<String, Object>>，并按 'timestamp' 字段降序排序。请使用 Stream API 和 Comparator。` | 语言特性、排序规则、流式处理。 |
| **Spring 组件** | `基于 Spring Boot 3，实现一个名为 'RateLimitAspect' 的 AOP 切面，用于接口限流，使用 Guava RateLimiter。` | 技术栈、设计模式、具体实现库。 |
| **并发处理** | `实现一个方法，接收一个 URL 列表，并使用 CompletableFuture 并行请求所有 URL，返回结果列表。` | 并发模型、异步编程、返回类型。 |

**代码示例（Java）：**

```java
// 提示词: 实现一个名为 'safeDivide' 的方法，接收两个 BigDecimal，
// 如果除数为 0，则抛出 IllegalArgumentException，并附带明确的错误信息。
// 结果保留 2 位小数，使用四舍五入。

public BigDecimal safeDivide(BigDecimal dividend, BigDecimal divisor) {
    // AI 生成的代码
    if (divisor.compareTo(BigDecimal.ZERO) == 0) {
        throw new IllegalArgumentException("除数不能为零");
    }
    return dividend.divide(divisor, 2, RoundingMode.HALF_UP);
}
```

### 4.2 场景 B：Bug 修复与堆栈分析

当遇到错误时，将错误信息和相关代码片段一起提供给 AI。

**操作步骤：**

1.  复制完整的**错误堆栈信息**（从终端、IDE 控制台或日志文件）。
2.  在 Cursor 中打开相关的代码文件。
3.  使用 `Cmd+L` 聊天，将堆栈信息粘贴进去，并使用 `@` 引用出错的文件。

**提示词模板：**

> "这是我的程序运行时的完整堆栈信息：`[粘贴堆栈信息]`。请分析错误原因，并告诉我应该如何修改 `@UserService.java` 中的 `createUser` 方法来修复它。**只输出修改后的代码和简短的解释。**"

**常见 Java 异常处理提示词：**

| 异常类型 | 提示词示例 |
| :--- | :--- |
| **NullPointerException** | `分析这个 NPE 堆栈，找出哪个变量为 null，并使用 Optional 或空值检查修复它。` |
| **ConcurrentModificationException** | `这个并发修改异常如何修复？请使用线程安全的集合或 CopyOnWriteArrayList。` |
| **TransactionException** | `分析这个事务回滚的原因，检查 @Transactional 注解的传播行为是否正确。` |

### 4.3 场景 C：代码重构与优化

| 优化目标 | 提示词模板 | 示例（Java） |
| :--- | :--- | :--- |
| **提高可读性** | `请将这段代码中的魔法数字（Magic Numbers）提取为常量，放置在类的顶部，使用 private static final。` | 将 `3600000` 提取为 `private static final long ONE_HOUR_MS = 3600000L;` |
| **性能优化** | `这段代码涉及大量字符串拼接，请使用 StringBuilder 进行优化。` | 将循环中的 `s += "a"` 优化为 `StringBuilder.append()`。 |
| **设计模式** | `请将这个类重构为单例模式（Singleton Pattern），使用枚举实现或双重检查锁定。` | 确保类只有一个实例，并提供全局访问点。 |
| **流式重构** | `请将这段 for 循环重构为 Stream API，使用 filter、map、collect。` | 提高代码可读性和函数式风格。 |

**示例（Java - Stream API 重构）：**

```java
// 原始代码
List<String> activeUserNames = new ArrayList<>();
for (User user : users) {
    if (user.getStatus() == UserStatus.ACTIVE) {
        activeUserNames.add(user.getName().toUpperCase());
    }
}

// 提示词: 请用 Stream API 重构这段代码
// AI 生成的代码
List<String> activeUserNames = users.stream()
    .filter(user -> user.getStatus() == UserStatus.ACTIVE)
    .map(user -> user.getName().toUpperCase())
    .collect(Collectors.toList());
```

### 4.4 场景 D：文档与测试生成

**生成文档（Javadoc）：**

*   **操作：** 将光标放在方法签名上，按下 `Cmd+K`。
*   **提示词：** `请为这个方法生成完整的 Javadoc，详细描述参数 @param、返回值 @return 和可能抛出的异常 @throws。`

**生成单元测试：**

*   **操作：** 在测试文件（如 `UserServiceTest.java`）中，使用 `@` 引用要测试的类。
*   **提示词：** `请为 @UserService.java 中的 'calculateDiscount' 方法编写 3 个单元测试用例，覆盖正常折扣、零折扣和非法输入（预期抛出异常）三种情况。请使用 JUnit 5 和 Mockito 框架，遵循 AAA（Arrange-Act-Assert）模式。`

**示例（JUnit 5 测试）：**

```java
// 提示词: 为 UserService.createUser 方法编写单元测试，
// 测试正常创建和用户名重复两种情况，使用 Mockito mock Repository

@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private UserRepository userRepository;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    @DisplayName("正常创建用户 - 应返回新用户ID")
    void createUser_WithValidInput_ShouldReturnUserId() {
        // Arrange
        CreateUserRequest request = new CreateUserRequest("john", "john@example.com");
        when(userRepository.existsByUsername("john")).thenReturn(false);
        when(userRepository.save(any(User.class))).thenReturn(new User(1L, "john"));
        
        // Act
        Long userId = userService.createUser(request);
        
        // Assert
        assertThat(userId).isEqualTo(1L);
        verify(userRepository).save(any(User.class));
    }
    
    @Test
    @DisplayName("用户名重复 - 应抛出异常")
    void createUser_WithDuplicateUsername_ShouldThrowException() {
        // Arrange
        CreateUserRequest request = new CreateUserRequest("john", "john@example.com");
        when(userRepository.existsByUsername("john")).thenReturn(true);
        
        // Act & Assert
        assertThrows(DuplicateUsernameException.class, 
            () -> userService.createUser(request));
    }
}
```

### 4.5 场景 E：Spring Boot 常见任务

| 任务 | 提示词模板 |
| :--- | :--- |
| **REST Controller** | `创建一个 UserController，包含 CRUD 接口，使用 @RestController、@RequestMapping，返回 ResponseEntity，遵循 RESTful 规范。` |
| **数据校验** | `为这个 DTO 添加 JSR-380 校验注解（@NotNull, @Size, @Email 等），并在 Controller 中使用 @Valid 触发校验。` |
| **异常处理** | `创建一个 @ControllerAdvice 全局异常处理类，处理 MethodArgumentNotValidException 和自定义业务异常，返回统一的错误响应格式。` |
| **配置类** | `创建一个 Spring Configuration 类，配置 RestTemplate Bean，设置连接超时和读取超时。` |

---

## 5. 高级技巧与效率提升

### 5.1 构建可复用的提示词模板

对于重复性的任务（如生成特定的测试模板、创建标准的 Service 结构），可以将提示词保存为文本片段或自定义命令。

**模板示例（用于创建 Spring Service）：**

> **[Spring Service Template]**
>
> `请创建一个名为 {ServiceName}Service 的 Spring Service 类。它应该：1) 使用 @Service 注解；2) 使用构造器注入 {RepositoryName}Repository；3) 包含基本的 CRUD 方法；4) 使用 @Transactional 注解管理事务。请遵循阿里巴巴 Java 开发规范。`

**模板示例（用于创建实体类）：**

> **[JPA Entity Template]**
>
> `请创建一个名为 {EntityName} 的 JPA 实体类。它应该：1) 使用 @Entity 和 @Table 注解；2) 主键使用 @Id 和 @GeneratedValue(strategy = GenerationType.IDENTITY)；3) 包含 {FieldList} 字段；4) 使用 Lombok 的 @Data、@Builder、@NoArgsConstructor、@AllArgsConstructor。`

### 5.2 利用工具的记忆/上下文管理

Cursor 等工具的聊天窗口通常具有**记忆功能**。这意味着在同一个聊天会话中，AI 会记住你之前讨论的内容和代码。

**最佳实践：**

*   **保持会话的专注性：** 针对一个复杂功能或 Bug 修复，始终在一个聊天会话中进行。这样，你后续的提示词可以更简洁（例如，只需说"现在请修改 save 方法"），因为 AI 已经理解了整个上下文。
*   **新任务，新会话：** 当开始一个全新的、不相关的任务时，应开启一个新的聊天会话，以避免旧的上下文干扰新的判断。

### 5.3 Java 生态系统特定技巧

| 场景 | 高效提示词技巧 |
| :--- | :--- |
| **Maven/Gradle 依赖** | `请告诉我实现 XX 功能需要添加哪些 Maven 依赖，给出完整的 <dependency> 标签。` |
| **注解驱动开发** | `请解释 @ConditionalOnProperty 注解的作用，并给出在配置类中的使用示例。` |
| **设计模式应用** | `请用工厂模式重构这段代码，根据 type 字段创建不同的 Handler 实现类。` |
| **响应式编程** | `请将这个阻塞式 Service 方法改写为响应式风格，使用 Spring WebFlux 的 Mono 和 Flux。` |

---

## 6. 避坑指南：常见错误与安全建议

### 6.1 过度依赖与盲目信任（幻觉识别）

AI 最大的陷阱是**幻觉（Hallucination）**——它会自信地生成看似正确但实际上完全错误的代码或信息。

| 陷阱 | 表现 | 应对策略 |
| :--- | :--- | :--- |
| **逻辑错误** | 代码通过编译，但在特定边界条件下失败。 | **必须**编写单元测试，并手动检查边界条件（如 null、空集合、并发场景）。 |
| **API 错误** | 引用了已废弃的 API 或不存在的方法（如 Java 8 之前的 Optional 用法）。 | 交叉验证官方 Javadoc 文档，尤其是涉及版本更新时。 |
| **安全漏洞** | 生成了易受 SQL 注入或 XSS 攻击的代码（如字符串拼接 SQL）。 | **始终**假设 AI 生成的代码存在安全风险，使用 PreparedStatement、参数化查询、Spring Security 等安全实践。 |
| **依赖版本** | 推荐了已过时或存在漏洞的依赖版本。 | 使用 `mvn dependency:check` 或 OWASP 依赖检查工具验证。 |

### 6.2 提示词过于模糊或缺乏必要上下文

这是导致低效交互的头号原因。

**低效提示词：** "帮我写点代码。"

**高效提示词：** "请在 `UserService.java` 中，实现一个名为 `formatBirthday` 的方法，它接收一个 LocalDate 对象，并返回 `yyyy年MM月dd日` 格式的字符串。使用 `DateTimeFormatter`。"

### 6.3 忽略安全性和隐私性

**敏感信息处理：**

*   **切勿**将真实的 API Key、数据库密码、JWT Secret 或敏感的业务逻辑直接粘贴到聊天框中。
*   如果需要 AI 帮助处理涉及敏感数据的代码，请使用**脱敏后的示例数据**或**抽象的变量名**（如 `${DB_PASSWORD}`）。
*   了解你使用的 AI 工具的数据隐私政策。例如，某些工具在企业模式下会保证代码不会用于模型训练。

### 6.4 Java 特有的注意事项

| 注意点 | 说明 | 建议 |
| :--- | :--- | :--- |
| **泛型擦除** | AI 可能生成在运行时因泛型擦除而失败的代码。 | 审查涉及泛型的反射代码，必要时使用 TypeToken。 |
| **线程安全** | AI 可能忽略并发场景下的线程安全问题。 | 对共享状态的代码，手动检查是否需要同步或使用并发集合。 |
| **资源释放** | AI 可能生成未正确关闭资源的代码。 | 确保使用 try-with-resources 或在 finally 中关闭资源。 |
| **受检异常** | AI 可能遗漏必要的异常处理。 | 检查方法签名中的 throws 声明是否完整。 |

---

## 7. 开发者心态：成为 AI 的"领航员"

AI 编程工具是生产力革命，但它要求开发者转变心态。

### 7.1 保持批判性思维

AI 让你从"代码的生产者"转变为"**代码的审核者和集成者**"。你的核心价值不再是敲击键盘的速度，而是**判断力、设计能力和对业务的深刻理解**。

> **"AI 让你更快地犯错，也更快地修正错误。"**

### 7.2 评估 AI 建议的有效性

在接受 AI 的建议之前，问自己以下三个问题：

1.  **它是否解决了我的业务问题？** (逻辑正确性)
2.  **它是否符合我的项目规范和技术栈？** (风格和兼容性)
3.  **它是否引入了新的复杂性或安全风险？** (质量和安全)

只有当三个问题的答案都是肯定的，你才能放心地集成 AI 生成的代码。将 AI 视为一个强大的助手，而你，始终是掌握方向的**领航员**。

---

## 附录：Java 开发常用提示词速查表

| 场景 | 提示词模板 |
| :--- | :--- |
| **创建实体类** | `创建 {EntityName} 实体，字段包括 {fields}，使用 JPA 注解和 Lombok。` |
| **创建 DTO** | `为 {EntityName} 创建 Request 和 Response DTO，使用 record 类型（Java 17+）。` |
| **创建 Repository** | `创建 {EntityName}Repository 接口，继承 JpaRepository，添加自定义查询方法。` |
| **创建 Service** | `创建 {ServiceName}Service，注入 Repository，实现 CRUD 方法，添加事务管理。` |
| **创建 Controller** | `创建 {ControllerName}Controller，RESTful 风格，使用 @RestController，返回 ResponseEntity。` |
| **异常处理** | `创建自定义异常 {ExceptionName} 和对应的 @ControllerAdvice 全局处理器。` |
| **单元测试** | `为 {ClassName} 的 {methodName} 方法编写 JUnit 5 测试，使用 Mockito，覆盖正常和异常场景。` |
| **集成测试** | `为 {ControllerName} 编写 @SpringBootTest 集成测试，使用 @MockMvc 测试 API 端点。` |

---

**参考文献**

[1] Cursor Documentation. *The Official Guide to Cursor Features and Best Practices.*
[2] OpenAI. *Prompt Engineering Guide.*
[3] GitHub. *GitHub Copilot Documentation.*
[4] 阿里巴巴. *阿里巴巴 Java 开发手册（嵩山版）.*
[5] Spring. *Spring Boot Reference Documentation.*
