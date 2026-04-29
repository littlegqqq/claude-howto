# 代码异味目录

基于 Martin Fowler 《重构》（第2版）的代码异味综合参考。代码异味是更深层问题的症状——它们表明代码设计可能有问题。

> "代码异味是一种表面迹象，通常对应于系统中更深层的问题。" — Martin Fowler

---

## 膨胀器

代表某些东西已经增长到无法有效处理的大小的代码异味。

### 过长方法

**迹象：**
- 方法超过 30-50 行
- 需要滚动才能看到整个方法
- 多层嵌套
- 注释解释各部分做什么

**为什么不好：**
- 难以理解
- 难以单独测试
- 更改有意外后果
- 重复逻辑隐藏在内部

**重构手法：**
- 提炼方法
- 以查询取代临时变量
- 引入参数对象
- 以方法对象取代方法
- 分解条件表达式

**示例（之前）：**
```javascript
function processOrder(order) {
  // Validate order (20 lines)
  if (!order.items) throw new Error('No items');
  if (order.items.length === 0) throw new Error('Empty order');
  // ... more validation

  // Calculate totals (30 lines)
  let subtotal = 0;
  for (const item of order.items) {
    subtotal += item.price * item.quantity;
  }
  // ... tax, shipping, discounts

  // Send notifications (20 lines)
  // ... email logic
}
```

**示例（之后）：**
```javascript
function processOrder(order) {
  validateOrder(order);
  const totals = calculateOrderTotals(order);
  sendOrderNotifications(order, totals);
  return { order, totals };
}
```

---

### 过大类

**迹象：**
- 类有许多实例变量（>7-10）
- 类有许多方法（>15-20）
- 类名模糊（Manager、Handler、Processor）
- 方法不使用所有实例变量

**为什么不好：**
- 违反单一职责原则
- 难以测试
- 更改波及无关功能
- 难以重用部分

**重构手法：**
- 提炼类
- 提炼子类
- 提炼接口

**检测：**
```
Lines of code > 300
Number of methods > 15
Number of fields > 10
```

---

### 基本类型偏执

**迹象：**
- 对领域概念使用基本类型（string 表示 email，int 表示金额）
- 用基本类型数组代替对象
- 用字符串常量表示类型码
- 魔法数字/字符串

**为什么不好：**
- 类型层面无验证
- 逻辑分散在代码库中
- 容易传递错误值
- 缺失领域概念

**重构手法：**
- 以对象取代基本类型
- 以类取代类型码
- 以子类取代类型码
- 以 State/Strategy 取代类型码

**示例（之前）：**
```javascript
const user = {
  email: 'john@example.com',     // Just a string
  phone: '1234567890',           // Just a string
  status: 'active',              // Magic string
  balance: 10050                 // Cents as integer
};
```

**示例（之后）：**
```javascript
const user = {
  email: new Email('john@example.com'),
  phone: new PhoneNumber('1234567890'),
  status: UserStatus.ACTIVE,
  balance: Money.cents(10050)
};
```

---

### 过长参数列表

**迹象：**
- 方法有 4+ 个参数
- 参数总是一起出现
- 布尔标志改变方法行为
- 经常传递 null/undefined

**为什么不好：**
- 难以正确调用
- 参数顺序混乱
- 表明方法做太多事情
- 难以添加新参数

**重构手法：**
- 引入参数对象
- 保持对象完整
- 以方法调用取代参数
- 移除标志参数

**示例（之前）：**
```javascript
function createUser(firstName, lastName, email, phone,
                    street, city, state, zip,
                    isAdmin, isActive, createdBy) {
  // ...
}
```

**示例（之后）：**
```javascript
function createUser(personalInfo, address, options) {
  // personalInfo: { firstName, lastName, email, phone }
  // address: { street, city, state, zip }
  // options: { isAdmin, isActive, createdBy }
}
```

---

### 数据泥团

**迹象：**
- 相同的 3+ 个字段反复一起出现
- 参数总是一起传递
- 类有属于一起的字段子集

**为什么不好：**
- 重复处理逻辑
- 缺失抽象
- 更难扩展
- 表明隐藏的类

**重构手法：**
- 提炼类
- 引入参数对象
- 保持对象完整

**示例：**
```javascript
// Data clump: (x, y, z) coordinates
function movePoint(x, y, z, dx, dy, dz) { }
function scalePoint(x, y, z, factor) { }
function distanceBetween(x1, y1, z1, x2, y2, z2) { }

// Extract Point3D class
class Point3D {
  constructor(x, y, z) { }
  move(delta) { }
  scale(factor) { }
  distanceTo(other) { }
}
```

---

## 面向对象滥用

表明不完整或不正确使用 OOP 原则的异味。

### Switch 语句

**迹象：**
- 长的 switch/case 或 if/else 链
- 相同的 switch 在多处出现
- 对类型码使用 switch
- 添加新情况需要到处更改

**为什么不好：**
- 违反开闭原则
- 更改波及所有 switch 位置
- 难以扩展
- 通常表明缺少多态

**重构手法：**
- 以多态取代条件表达式
- 以子类取代类型码
- 以 State/Strategy 取代类型码

**示例（之前）：**
```javascript
function calculatePay(employee) {
  switch (employee.type) {
    case 'hourly':
      return employee.hours * employee.rate;
    case 'salaried':
      return employee.salary / 12;
    case 'commissioned':
      return employee.sales * employee.commission;
  }
}
```

**示例（之后）：**
```javascript
class HourlyEmployee {
  calculatePay() {
    return this.hours * this.rate;
  }
}

class SalariedEmployee {
  calculatePay() {
    return this.salary / 12;
  }
}
```

---

### 临时字段

**迹象：**
- 实例变量只在某些方法中使用
- 字段有条件地设置
- 某些情况下复杂的初始化

**为什么不好：**
- 令人困惑——字段存在但可能为 null
- 难以理解对象状态
- 表明条件逻辑隐藏

**重构手法：**
- 提炼类
- 引入 Null 对象
- 以局部变量取代临时字段

---

### 被拒绝的遗赠

**迹象：**
- 子类不使用继承的方法/数据
- 子类覆盖为什么都不做
- 继承用于代码重用，而非 IS-A 关系

**为什么不好：**
- 错误的抽象
- 违反里氏替换原则
- 误导性的层次结构

**重构手法：**
- 下移方法/字段
- 以委托取代子类
- 以委托取代继承

---

### 具有不同接口的替代类

**迹象：**
- 两个类做类似的事情
- 相同概念使用不同方法名
- 可以互换使用

**为什么不好：**
- 重复实现
- 没有共同接口
- 难以在两者之间切换

**重构手法：**
- 重命名方法
- 搬移方法
- 提炼超类
- 提炼接口

---

## 变更阻碍者

使更改变得困难的异味——更改一件事需要更改许多其他东西。

### 发散式变化

**迹象：**
- 一个类因为多种不同原因而更改
- 不同领域的更改触发相同类的编辑
- 类是"上帝类"

**为什么不好：**
- 违反单一职责
- 高变更频率
- 合并冲突

**重构手法：**
- 提炼类
- 提炼超类
- 提炼子类

**示例：**
一个 `User` 类因以下原因更改：
- 认证更改
- 个人资料更改
- 计费更改
- 通知更改

→ 提取：`AuthService`、`ProfileService`、`BillingService`、`NotificationService`

---

### 霰弹式修改

**迹象：**
- 一个更改需要编辑许多类
- 小功能需要触及 10+ 个文件
- 更改分散，难以找到全部

**为什么不好：**
- 容易遗漏
- 高耦合
- 更改容易出错

**重构手法：**
- 搬移方法
- 搬移字段
- 内联类

**检测：**
寻找：添加一个字段需要更改 >5 个文件。

---

### 平行继承体系

**迹象：**
- 在一个层次结构中创建子类需要在另一个中也创建子类
- 类前缀匹配（例如，`DatabaseOrder`、`DatabaseProduct`）

**为什么不好：**
- 双倍的维护
- 层次结构之间的耦合
- 容易忘记一边

**重构手法：**
- 搬移方法
- 搬移字段
- 消除一个层次结构

---

## 可有可无者

不必要的东西，应该被移除。

### 注释（过多的）

**迹象：**
- 注释解释代码做什么
- 注释掉的代码
- TODO/FIXME 永远不处理
- 注释中的道歉

**为什么不好：**
- 注释会撒谎（与代码不同步）
- 代码应该自文档化
- 死代码引起困惑

**重构手法：**
- 提炼方法（名称解释做什么）
- 重命名（无需注释的清晰度）
- 移除注释掉的代码
- 引入断言

**好注释 vs 坏注释：**
```javascript
// BAD: Explaining what
// Loop through users and check if active
for (const user of users) {
  if (user.status === 'active') { }
}

// GOOD: Explaining why
// Active users only - inactive are handled by cleanup job
const activeUsers = users.filter(u => u.isActive);
```

---

### 重复代码

**迹象：**
- 相同的代码在多处出现
- 类似的代码有小变化
- 复制粘贴模式

**为什么不好：**
- Bug 修复需要在多处进行
- 不一致性风险
- 代码库膨胀

**重构手法：**
- 提炼方法
- 提炼类
- 上移方法（在层次结构中）
- 塑造模板方法

**检测规则：**
任何重复 3+ 次的代码应该被提取。

---

### 冗赘类

**迹象：**
- 类做的事情不足以证明其存在
- 没有附加价值的包装器
- 过度工程化的结果

**为什么不好：**
- 维护开销
- 不必要的间接性
- 没有好处的复杂性

**重构手法：**
- 内联类
- 折叠继承体系

---

### 死代码

**迹象：**
- 不可达的代码
- 未使用的变量/方法/类
- 注释掉的代码
- 不可能条件后面的代码

**为什么不好：**
- 困惑
- 维护负担
- 减慢理解速度

**重构手法：**
- 移除死代码
- 安全删除

**检测：**
```bash
# Look for unused exports
# Look for unreferenced functions
# IDE "unused" warnings
```

---

### 投机性通用

**迹象：**
- 只有一个子类的抽象类
- "未来使用"的未使用参数
- 只做委托的方法
- 只有一个用例的"框架"

**为什么不好：**
- 没有好处的复杂性
- YAGNI（你不会需要它）
- 更难理解

**重构手法：**
- 折叠继承体系
- 内联类
- 移除参数
- 重命名方法

---

## 耦合器

代表类之间过度耦合的异味。

### 特性羡慕

**迹象：**
- 方法使用另一个类的数据多于自己的
- 对另一个对象大量调用 getter
- 数据和行为分离

**为什么不好：**
- 行为位置错误
- 封装不良
- 难以维护

**重构手法：**
- 搬移方法
- 搬移字段
- 提炼方法（然后搬移）

**示例（之前）：**
```javascript
class Order {
  getDiscountedPrice(customer) {
    // Uses customer data heavily
    if (customer.loyaltyYears > 5) {
      return this.price * customer.discountRate;
    }
    return this.price;
  }
}
```

**示例（之后）：**
```javascript
class Customer {
  getDiscountedPriceFor(price) {
    if (this.loyaltyYears > 5) {
      return price * this.discountRate;
    }
    return price;
  }
}
```

---

### 不当亲密

**迹象：**
- 类访问彼此的私有部分
- 双向引用
- 子类对父类了解太多

**为什么不好：**
- 高耦合
- 更改级联
- 难以修改一个而不影响另一个

**重构手法：**
- 搬移方法
- 搬移字段
- 将双向改为单向
- 提炼类
- 隐藏委托

---

### 消息链

**迹象：**
- 长链方法调用：`a.getB().getC().getD().getValue()`
- 客户端依赖导航结构
- "火车残骸"代码

**为什么不好：**
- 脆弱——任何更改都会断链
- 违反迪米特法则
- 对结构的耦合

**重构手法：**
- 隐藏委托
- 提炼方法
- 搬移方法

**示例：**
```javascript
// Bad: Message chain
const managerName = employee.getDepartment().getManager().getName();

// Better: Hide delegation
const managerName = employee.getManagerName();
```

---

### 中间人

**迹象：**
- 类只委托给另一个
- 一半的方法是委托
- 没有附加价值

**为什么不好：**
- 不必要的间接性
- 维护开销
- 令人困惑的架构

**重构手法：**
- 移除中间人
- 内联方法

---

## 异味严重程度指南

| 严重程度 | 描述 | 行动 |
|----------|-------------|--------|
| **严重** | 阻塞开发，导致 bug | 立即修复 |
| **高** | 显著的维护负担 | 在当前迭代中修复 |
| **中** | 明显但可管理 | 近期计划修复 |
| **低** | 轻微不便 | 机会性修复 |

---

## 快速检测清单

扫描代码时使用此清单：

- [ ] 任何方法超过 30 行？
- [ ] 任何类超过 300 行？
- [ ] 任何方法有超过 4 个参数？
- [ ] 任何重复的代码块？
- [ ] 任何对类型码的 switch/case？
- [ ] 任何未使用的代码？
- [ ] 任何方法大量使用另一个类的数据？
- [ ] 任何长链方法调用？
- [ ] 任何注释解释"是什么"而不是"为什么"？
- [ ] 任何应该是对象的基本类型？

---

## 延伸阅读

- Fowler, M. (2018). *Refactoring: Improving the Design of Existing Code* (2nd ed.)
- Kerievsky, J. (2004). *Refactoring to Patterns*
- Feathers, M. (2004). *Working Effectively with Legacy Code*
