# 重构目录

来自 Martin Fowler《重构》（第2版）的精选重构技术目录。每个重构包含动机、分步机制和示例。

> "重构由其机制定义——你执行更改时遵循的精确步骤序列。" — Martin Fowler

---

## 如何使用此目录

1. 使用代码异味参考**识别异味**
2. 在此目录中**找到匹配的重构**
3. **按步骤遵循机制**
4. **每一步后测试**以确保行为保持

**黄金法则**：如果任何步骤需要超过 10 分钟，将其分解为更小的步骤。

---

## 最常见的重构

### 提炼方法（Extract Method）

**何时使用**：过长方法、重复代码、需要命名一个概念

**动机**：将代码片段转变为一个名称解释其目的的方法。

**机制**：
1. 创建新方法，以它做什么命名（而非怎么做）
2. 将代码片段复制到新方法
3. 扫描片段中使用的局部变量
4. 将局部变量作为参数传递（或在方法中声明）
5. 适当处理返回值
6. 将原始片段替换为对新方法的调用
7. 测试

**之前**：
```javascript
function printOwing(invoice) {
  let outstanding = 0;

  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");

  // Calculate outstanding
  for (const order of invoice.orders) {
    outstanding += order.amount;
  }

  // Print details
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```

**之后**：
```javascript
function printOwing(invoice) {
  printBanner();
  const outstanding = calculateOutstanding(invoice);
  printDetails(invoice, outstanding);
}

function printBanner() {
  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");
}

function calculateOutstanding(invoice) {
  return invoice.orders.reduce((sum, order) => sum + order.amount, 0);
}

function printDetails(invoice, outstanding) {
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```

---

### 内联方法（Inline Method）

**何时使用**：方法体与方法名同样清晰、过度委托

**动机**：当方法不增加价值时移除不必要的间接性。

**机制**：
1. 检查方法不是多态的
2. 找到所有调用
3. 将每个调用替换为方法体
4. 每次替换后测试
5. 移除方法定义

**之前**：
```javascript
function getRating(driver) {
  return moreThanFiveLateDeliveries(driver) ? 2 : 1;
}

function moreThanFiveLateDeliveries(driver) {
  return driver.numberOfLateDeliveries > 5;
}
```

**之后**：
```javascript
function getRating(driver) {
  return driver.numberOfLateDeliveries > 5 ? 2 : 1;
}
```

---

### 提炼变量（Extract Variable）

**何时使用**：复杂表达式难以理解

**动机**：给复杂表达式的一部分命名。

**机制**：
1. 确保表达式没有副作用
2. 声明一个不可变变量
3. 将其设为表达式（或部分）的结果
4. 将原始表达式替换为变量
5. 测试

**之前**：
```javascript
return order.quantity * order.itemPrice -
  Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
  Math.min(order.quantity * order.itemPrice * 0.1, 100);
```

**之后**：
```javascript
const basePrice = order.quantity * order.itemPrice;
const quantityDiscount = Math.max(0, order.quantity - 500) * order.itemPrice * 0.05;
const shipping = Math.min(basePrice * 0.1, 100);
return basePrice - quantityDiscount + shipping;
```

---

### 内联变量（Inline Variable）

**何时使用**：变量名不比表达式传达更多信息

**动机**：移除不必要的间接性。

**机制**：
1. 检查右侧没有副作用
2. 如果变量不是不可变的，使其不可变并测试
3. 找到第一个引用并替换为表达式
4. 测试
5. 对所有引用重复
6. 移除声明和赋值
7. 测试

---

### 重命名变量（Rename Variable）

**何时使用**：名称没有清晰传达目的

**动机**：好的名称对整洁代码至关重要。

**机制**：
1. 如果变量广泛使用，考虑封装
2. 找到所有引用
3. 更改每个引用
4. 测试

**提示**：
- 使用揭示意图的名称
- 避免缩写
- 使用领域术语

```javascript
// Bad
const d = 30;
const x = users.filter(u => u.a);

// Good
const daysSinceLastLogin = 30;
const activeUsers = users.filter(user => user.isActive);
```

---

### 改变函数声明（Change Function Declaration）

**何时使用**：函数名不解释目的、参数需要更改

**动机**：好的函数名使代码自文档化。

**机制（简单）**：
1. 移除不需要的参数
2. 更改名称
3. 添加需要的参数
4. 测试

**机制（迁移 - 用于复杂更改）**：
1. 如果移除参数，确保它未被使用
2. 创建具有所需声明的新函数
3. 让旧函数调用新函数
4. 测试
5. 将调用者更改为使用新函数
6. 每次后测试
7. 移除旧函数

**之前**：
```javascript
function circum(radius) {
  return 2 * Math.PI * radius;
}
```

**之后**：
```javascript
function circumference(radius) {
  return 2 * Math.PI * radius;
}
```

---

### 封装变量（Encapsulate Variable）

**何时使用**：多处直接访问数据

**动机**：为数据操作提供清晰的访问点。

**机制**：
1. 创建 getter 和 setter 函数
2. 找到所有引用
3. 将读取替换为 getter
4. 将写入替换为 setter
5. 每次更改后测试
6. 限制变量的可见性

**之前**：
```javascript
let defaultOwner = { firstName: "Martin", lastName: "Fowler" };

// Used in many places
spaceship.owner = defaultOwner;
```

**之后**：
```javascript
let defaultOwnerData = { firstName: "Martin", lastName: "Fowler" };

function defaultOwner() { return defaultOwnerData; }
function setDefaultOwner(arg) { defaultOwnerData = arg; }

spaceship.owner = defaultOwner();
```

---

### 引入参数对象（Introduce Parameter Object）

**何时使用**：多个参数经常一起出现

**动机**：将自然属于一起的数据分组。

**机制**：
1. 为分组参数创建新的类/结构
2. 测试
3. 使用改变函数声明添加新对象
4. 测试
5. 对于组中的每个参数，从函数中移除它并使用新对象
6. 每次后测试

**之前**：
```javascript
function amountInvoiced(startDate, endDate) { ... }
function amountReceived(startDate, endDate) { ... }
function amountOverdue(startDate, endDate) { ... }
```

**之后**：
```javascript
class DateRange {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }
}

function amountInvoiced(dateRange) { ... }
function amountReceived(dateRange) { ... }
function amountOverdue(dateRange) { ... }
```

---

### 将函数组合成类（Combine Functions into Class）

**何时使用**：多个函数操作相同的数据

**动机**：将函数与它们操作的数据分组。

**机制**：
1. 对公共数据应用封装记录
2. 将每个函数移入类
3. 每次移动后测试
4. 将数据参数替换为类字段的使用

**之前**：
```javascript
function base(reading) { ... }
function taxableCharge(reading) { ... }
function calculateBaseCharge(reading) { ... }
```

**之后**：
```javascript
class Reading {
  constructor(data) { this._data = data; }

  get base() { ... }
  get taxableCharge() { ... }
  get calculateBaseCharge() { ... }
}
```

---

### 拆分阶段（Split Phase）

**何时使用**：代码处理两件不同的事情

**动机**：将代码分成具有清晰边界的不同阶段。

**机制**：
1. 为第二阶段创建第二个函数
2. 测试
3. 在阶段之间引入中间数据结构
4. 测试
5. 将第一阶段提取到自己的函数
6. 测试

**之前**：
```javascript
function priceOrder(product, quantity, shippingMethod) {
  const basePrice = product.basePrice * quantity;
  const discount = Math.max(quantity - product.discountThreshold, 0)
    * product.basePrice * product.discountRate;
  const shippingPerCase = (basePrice > shippingMethod.discountThreshold)
    ? shippingMethod.discountedFee : shippingMethod.feePerCase;
  const shippingCost = quantity * shippingPerCase;
  return basePrice - discount + shippingCost;
}
```

**之后**：
```javascript
function priceOrder(product, quantity, shippingMethod) {
  const priceData = calculatePricingData(product, quantity);
  return applyShipping(priceData, shippingMethod);
}

function calculatePricingData(product, quantity) {
  const basePrice = product.basePrice * quantity;
  const discount = Math.max(quantity - product.discountThreshold, 0)
    * product.basePrice * product.discountRate;
  return { basePrice, quantity, discount };
}

function applyShipping(priceData, shippingMethod) {
  const shippingPerCase = (priceData.basePrice > shippingMethod.discountThreshold)
    ? shippingMethod.discountedFee : shippingMethod.feePerCase;
  const shippingCost = priceData.quantity * shippingPerCase;
  return priceData.basePrice - priceData.discount + shippingCost;
}
```

---

## 搬移特性

### 搬移方法（Move Method）

**何时使用**：方法使用另一个类的特性多于自己的

**动机**：将函数放在它们最多使用的数据旁边。

**机制**：
1. 检查方法在其类中使用的所有程序元素
2. 检查方法是否是多态的
3. 将方法复制到目标类
4. 针对新上下文进行调整
5. 让原始方法委托给目标
6. 测试
7. 考虑移除原始方法

---

### 搬移字段（Move Field）

**何时使用**：字段被另一个类更多地使用

**动机**：将数据与使用它的函数放在一起。

**机制**：
1. 如果尚未封装，封装字段
2. 测试
3. 在目标中创建字段
4. 更新引用以使用目标字段
5. 测试
6. 移除原始字段

---

### 将语句移入函数（Move Statements into Function）

**何时使用**：相同的代码总是与函数调用一起出现

**动机**：通过将重复代码移入函数来消除重复。

**机制**：
1. 如果尚未提取，将重复代码提取为函数
2. 将语句移入该函数
3. 测试
4. 如果调用者不再需要独立语句，移除它们

---

### 将语句移到调用者（Move Statements to Callers）

**何时使用**：公共行为在调用者之间有所不同

**动机**：当行为需要不同时，将其从函数中移出。

**机制**：
1. 对要移动的代码使用提炼方法
2. 对原始函数使用内联方法
3. 移除现在内联的调用
4. 将提取的代码移到每个调用者
5. 测试

---

## 组织数据

### 以对象取代基本类型（Replace Primitive with Object）

**何时使用**：数据项需要比简单值更多的行为

**动机**：用其行为封装数据。

**机制**：
1. 应用封装变量
2. 创建简单的值类
3. 更改 setter 以创建新实例
4. 更改 getter 以返回值
5. 测试
6. 向新类添加更丰富的行为

**之前**：
```javascript
class Order {
  constructor(data) {
    this.priority = data.priority; // string: "high", "rush", etc.
  }
}

// Usage
if (order.priority === "high" || order.priority === "rush") { ... }
```

**之后**：
```javascript
class Priority {
  constructor(value) {
    if (!Priority.legalValues().includes(value))
      throw new Error(`Invalid priority: ${value}`);
    this._value = value;
  }

  static legalValues() { return ['low', 'normal', 'high', 'rush']; }
  get value() { return this._value; }

  higherThan(other) {
    return Priority.legalValues().indexOf(this._value) >
           Priority.legalValues().indexOf(other._value);
  }
}

// Usage
if (order.priority.higherThan(new Priority("normal"))) { ... }
```

---

### 以查询取代临时变量（Replace Temp with Query）

**何时使用**：临时变量持有表达式的结果

**动机**：通过将表达式提取为函数使代码更清晰。

**机制**：
1. 检查变量只被赋值一次
2. 将赋值的右侧提取为方法
3. 将对临时变量的引用替换为方法调用
4. 测试
5. 移除临时变量的声明和赋值

**之前**：
```javascript
const basePrice = this._quantity * this._itemPrice;
if (basePrice > 1000) {
  return basePrice * 0.95;
} else {
  return basePrice * 0.98;
}
```

**之后**：
```javascript
get basePrice() {
  return this._quantity * this._itemPrice;
}

// In the method
if (this.basePrice > 1000) {
  return this.basePrice * 0.95;
} else {
  return this.basePrice * 0.98;
}
```

---

## 简化条件逻辑

### 分解条件表达式（Decompose Conditional）

**何时使用**：复杂的条件（if-then-else）语句

**动机**：通过提取条件和动作使意图清晰。

**机制**：
1. 对条件应用提炼方法
2. 对 then 分支应用提炼方法
3. 对 else 分支应用提炼方法（如存在）

**之前**：
```javascript
if (!aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)) {
  charge = quantity * plan.summerRate;
} else {
  charge = quantity * plan.regularRate + plan.regularServiceCharge;
}
```

**之后**：
```javascript
if (isSummer(aDate, plan)) {
  charge = summerCharge(quantity, plan);
} else {
  charge = regularCharge(quantity, plan);
}

function isSummer(date, plan) {
  return !date.isBefore(plan.summerStart) && !date.isAfter(plan.summerEnd);
}

function summerCharge(quantity, plan) {
  return quantity * plan.summerRate;
}

function regularCharge(quantity, plan) {
  return quantity * plan.regularRate + plan.regularServiceCharge;
}
```

---

### 合并条件表达式（Consolidate Conditional Expression）

**何时使用**：多个条件产生相同结果

**动机**：明确条件是单一检查。

**机制**：
1. 验证条件中没有副作用
2. 使用 `and` 或 `or` 合并条件
3. 考虑对合并后的条件应用提炼方法

**之前**：
```javascript
if (employee.seniority < 2) return 0;
if (employee.monthsDisabled > 12) return 0;
if (employee.isPartTime) return 0;
```

**之后**：
```javascript
if (isNotEligibleForDisability(employee)) return 0;

function isNotEligibleForDisability(employee) {
  return employee.seniority < 2 ||
         employee.monthsDisabled > 12 ||
         employee.isPartTime;
}
```

---

### 以卫语句取代嵌套条件（Replace Nested Conditional with Guard Clauses）

**何时使用**：深层嵌套条件使流程难以跟踪

**动机**：对特殊情况使用卫语句，保持正常流程清晰。

**机制**：
1. 找到特殊情况条件
2. 将它们替换为提前返回的卫语句
3. 每次更改后测试

**之前**：
```javascript
function payAmount(employee) {
  let result;
  if (employee.isSeparated) {
    result = { amount: 0, reasonCode: "SEP" };
  } else {
    if (employee.isRetired) {
      result = { amount: 0, reasonCode: "RET" };
    } else {
      result = calculateNormalPay(employee);
    }
  }
  return result;
}
```

**之后**：
```javascript
function payAmount(employee) {
  if (employee.isSeparated) return { amount: 0, reasonCode: "SEP" };
  if (employee.isRetired) return { amount: 0, reasonCode: "RET" };
  return calculateNormalPay(employee);
}
```

---

### 以多态取代条件表达式（Replace Conditional with Polymorphism）

**何时使用**：基于类型的 switch/case、按类型变化的条件逻辑

**动机**：让对象处理自己的行为。

**机制**：
1. 创建类层次结构（如果不存在）
2. 使用工厂函数创建对象
3. 将条件逻辑移入超类方法
4. 为每个情况创建子类方法
5. 移除原始条件

**之前**：
```javascript
function plumages(birds) {
  return birds.map(b => plumage(b));
}

function plumage(bird) {
  switch (bird.type) {
    case 'EuropeanSwallow':
      return "average";
    case 'AfricanSwallow':
      return (bird.numberOfCoconuts > 2) ? "tired" : "average";
    case 'NorwegianBlueParrot':
      return (bird.voltage > 100) ? "scorched" : "beautiful";
    default:
      return "unknown";
  }
}
```

**之后**：
```javascript
class Bird {
  get plumage() { return "unknown"; }
}

class EuropeanSwallow extends Bird {
  get plumage() { return "average"; }
}

class AfricanSwallow extends Bird {
  get plumage() {
    return (this.numberOfCoconuts > 2) ? "tired" : "average";
  }
}

class NorwegianBlueParrot extends Bird {
  get plumage() {
    return (this.voltage > 100) ? "scorched" : "beautiful";
  }
}

function createBird(data) {
  switch (data.type) {
    case 'EuropeanSwallow': return new EuropeanSwallow(data);
    case 'AfricanSwallow': return new AfricanSwallow(data);
    case 'NorwegianBlueParrot': return new NorwegianBlueParrot(data);
    default: return new Bird(data);
  }
}
```

---

### 引入特殊情况（Null 对象）（Introduce Special Case）

**何时使用**：重复的 null 检查处理特殊情况

**动机**：返回处理特殊情况的特殊对象。

**机制**：
1. 创建具有预期接口的特殊情况类
2. 添加 isSpecialCase 检查
3. 引入工厂方法
4. 将 null 检查替换为特殊情况对象的使用
5. 测试

**之前**：
```javascript
const customer = site.customer;
// ... many places checking
if (customer === "unknown") {
  customerName = "occupant";
} else {
  customerName = customer.name;
}
```

**之后**：
```javascript
class UnknownCustomer {
  get name() { return "occupant"; }
  get billingPlan() { return registry.defaultPlan; }
}

// Factory method
function customer(site) {
  return site.customer === "unknown"
    ? new UnknownCustomer()
    : site.customer;
}

// Usage - no null checks needed
const customerName = customer.name;
```

---

## 重构 API

### 将查询与修改分离（Separate Query from Modifier）

**何时使用**：函数既返回值又有副作用

**动机**：明确哪些操作有副作用。

**机制**：
1. 创建新的查询函数
2. 复制原始函数的返回逻辑
3. 修改原始函数为返回 void
4. 替换使用返回值的调用
5. 测试

**之前**：
```javascript
function alertForMiscreant(people) {
  for (const p of people) {
    if (p === "Don") {
      setOffAlarms();
      return "Don";
    }
    if (p === "John") {
      setOffAlarms();
      return "John";
    }
  }
  return "";
}
```

**之后**：
```javascript
function findMiscreant(people) {
  for (const p of people) {
    if (p === "Don") return "Don";
    if (p === "John") return "John";
  }
  return "";
}

function alertForMiscreant(people) {
  if (findMiscreant(people) !== "") setOffAlarms();
}
```

---

### 参数化函数（Parameterize Function）

**何时使用**：多个函数做类似事情但使用不同值

**动机**：通过添加参数消除重复。

**机制**：
1. 选择一个函数
2. 为变化的字面量添加参数
3. 更改函数体使用参数
4. 测试
5. 更改调用者使用参数化版本
6. 移除现在未使用的函数

**之前**：
```javascript
function tenPercentRaise(person) {
  person.salary = person.salary * 1.10;
}

function fivePercentRaise(person) {
  person.salary = person.salary * 1.05;
}
```

**之后**：
```javascript
function raise(person, factor) {
  person.salary = person.salary * (1 + factor);
}

// Usage
raise(person, 0.10);
raise(person, 0.05);
```

---

### 移除标志参数（Remove Flag Argument）

**何时使用**：布尔参数改变函数行为

**动机**：通过单独的函数使行为明确。

**机制**：
1. 为每个标志值创建明确的函数
2. 将每个调用替换为适当的新函数
3. 每次更改后测试
4. 移除原始函数

**之前**：
```javascript
function bookConcert(customer, isPremium) {
  if (isPremium) {
    // premium booking logic
  } else {
    // regular booking logic
  }
}

bookConcert(customer, true);
bookConcert(customer, false);
```

**之后**：
```javascript
function bookPremiumConcert(customer) {
  // premium booking logic
}

function bookRegularConcert(customer) {
  // regular booking logic
}

bookPremiumConcert(customer);
bookRegularConcert(customer);
```

---

## 处理继承

### 上移方法（Pull Up Method）

**何时使用**：多个子类中相同的方法

**动机**：消除类层次结构中的重复。

**机制**：
1. 检查方法确保它们相同
2. 检查签名是否相同
3. 在超类中创建新方法
4. 从一个子类复制方法体
5. 删除一个子类方法，测试
6. 删除其他子类方法，每次测试

---

### 下移方法（Push Down Method）

**何时使用**：行为只与子类的子集相关

**动机**：将方法放在使用它的地方。

**机制**：
1. 将方法复制到需要它的每个子类
2. 从超类中移除方法
3. 测试
4. 从不需要它的子类中移除
5. 测试

---

### 以委托取代子类（Replace Subclass with Delegate）

**何时使用**：继承使用不正确，需要更多灵活性

**动机**：在适当时优先使用组合而非继承。

**机制**：
1. 为委托创建空类
2. 在宿主类中添加持有委托的字段
3. 创建委托的构造函数，从宿主调用
4. 将特性移到委托
5. 每次移动后测试
6. 将继承替换为委托

---

## 提炼类（Extract Class）

**何时使用**：大类有多个职责

**动机**：拆分类以保持单一职责。

**机制**：
1. 决定如何拆分职责
2. 创建新类
3. 将字段从原始类移到新类
4. 测试
5. 将方法从原始类移到新类
6. 每次移动后测试
7. 审查并重命名两个类
8. 决定如何暴露新类

**之前**：
```javascript
class Person {
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get officeAreaCode() { return this._officeAreaCode; }
  set officeAreaCode(arg) { this._officeAreaCode = arg; }
  get officeNumber() { return this._officeNumber; }
  set officeNumber(arg) { this._officeNumber = arg; }

  get telephoneNumber() {
    return `(${this._officeAreaCode}) ${this._officeNumber}`;
  }
}
```

**之后**：
```javascript
class Person {
  constructor() {
    this._telephoneNumber = new TelephoneNumber();
  }
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get telephoneNumber() { return this._telephoneNumber.toString(); }
  get officeAreaCode() { return this._telephoneNumber.areaCode; }
  set officeAreaCode(arg) { this._telephoneNumber.areaCode = arg; }
}

class TelephoneNumber {
  get areaCode() { return this._areaCode; }
  set areaCode(arg) { this._areaCode = arg; }
  get number() { return this._number; }
  set number(arg) { this._number = arg; }
  toString() { return `(${this._areaCode}) ${this._number}`; }
}
```

---

## 快速参考：异味到重构

| 代码异味 | 主要重构 | 替代方案 |
|------------|-------------------|-------------|
| 过长方法 | 提炼方法 | 以查询取代临时变量 |
| 重复代码 | 提炼方法 | 上移方法 |
| 过大类 | 提炼类 | 提炼子类 |
| 过长参数列表 | 引入参数对象 | 保持对象完整 |
| 特性羡慕 | 搬移方法 | 提炼方法 + 搬移 |
| 数据泥团 | 提炼类 | 引入参数对象 |
| 基本类型偏执 | 以对象取代基本类型 | 取代类型码 |
| Switch 语句 | 以多态取代条件表达式 | 取代类型码 |
| 临时字段 | 提炼类 | 引入 Null 对象 |
| 消息链 | 隐藏委托 | 提炼方法 |
| 中间人 | 移除中间人 | 内联方法 |
| 发散式变化 | 提炼类 | 拆分阶段 |
| 霰弹式修改 | 搬移方法 | 内联类 |
| 死代码 | 移除死代码 | - |
| 投机性通用 | 折叠继承体系 | 内联类 |

---

## 延伸阅读

- Fowler, M. (2018). *Refactoring: Improving the Design of Existing Code* (2nd ed.)
- 在线目录：https://refactoring.com/catalog/
