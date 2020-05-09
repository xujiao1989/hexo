---
title: Angular表单 -- FormControl、FormArray API走读
date: 2019-04-19 16:22:50
tags:
  - 前端开发
  - javascript
  - Angular7
categories: Angular7
---

本文来自angular官方文档。主要熟悉Angular Form的相关API

## AbstractControl

这个是这是 FormControl、FormGroup 和 FormArray 的基类，它有很多的属性和方法。它有两个参数：validator（	
用于决定该控件有效性的同步函数）、asyncValidator（	
用于决定该控件有效性的异步函数）。

### 属性有哪些（19个）

+ **value: any**
    + 只读
    + 控件的当前值
</br>

+ **validator: ValidatorFn | null**
    + Declared in constructor.
    + 用于决定该控件有效性的同步函数。
</br>

+ **asyncValidator: AsyncValidatorFn | null**
    + Declared in constructor.
    + 用于决定该控件有效性的异步函数。
</br>
+ **parent: FormGroup | FormArray**
    + 只读
    + 父控件
</br>

+ **status: string**
    + 只读：控件的有效性状态。有四个可能的值：
        + VALID: 该控件通过了所有有效性检查。
        + INVALID 该控件至少有一个有效性检查失败了。
        + PENDING：该控件正在进行有效性检查，处于中间状态。
        + DISABLED：该控件被禁用，豁免了有效性检查。
    + 这些状态值是互斥的，因此一个控件不可能同时处于有效状态和无效状态或无效状态和禁用状态。
</br>

+ **valid: boolean**
	+ 只读
    + 当控件的 status 为 VALID 时，它就是 valid 的。
</br>

+ **invalid: boolean**
    + 只读
    + 当控件的 status 为 INVALID 时，它就是 invalid 的。
</br>

+ **pending: boolean**
    + 只读
    + 当控件的 status 为 PENDING 时，它就是 pending 的。
</br>

+ **disabled: boolean**
    + 只读
    + 当控件的 status 为 DISABLED 时，它就是 disabled 的。
</br>

+ **enabled: boolean**
    + 只读
    + 当控件的 status 不是 DISABLED 时，它就是 enabled 的。
</br>

+ **errors: ValidationErrors | null**
    + 只读
    + 一个对象，包含由失败的验证所生成的那些错误，如果没出错则为 null。
</br>

+ **pristine: boolean**
    + 只读
    + 如果用户尚未修改 UI 中的值，则该控件是 pristine（原始状态）的
</br>

+ **dirty: boolean**
    + 只读
    + 如果用户修改过 UI 中的值，则控件是 dirty（脏） 的。
</br>

+ **touched: boolean**
	+ 只读
    + 如果控件被标记为 touched（碰过） 则为 true。
    + 一旦用户在控件上触发了 blur 事件，则会将其标记为 touched。 
</br>

+ **untouched: boolean**
	+ 只读
    + 如果该控件尚未标记为 touched，则为 true。
    + 如果用户尚未在控件上触发过 blur 事件，则该控件为 untouched。
</br>

+ **valueChanges: Observable<any>**
	+ 只读
    + 一个可观察对象Observable（可以去订阅它），每当控件的值发生变化时，它就会发出一个事件 —— 无论是通过 UI 还是通过程序。
</br>

+ **statusChanges: Observable<any>**
	+ 只读
    + 一个可观察对象Observable，每当控件的验证 status 被重新计算时，就会发出一个事件。
</br>

+ **updateOn: FormHooks**
    + 只读
    + 报告这个 AbstractControl 的更新策略（表示控件用来更新自身状态的事件）。 可能的值有 'change' | 'blur' | 'submit'，默认值是 'change'。
</br>

+ **root: AbstractControl**	
    + 只读
    + 获取该控件的顶级祖先。
</br>

### 方法有哪些（21个）

+ **setValidators()**
    + 设置该控件上所激活的同步验证器。调用它将会覆盖所有现存的同步验证器。
    + setValidators(newValidator: ValidatorFn | ValidatorFn[]): void
    + 参数：newValidator	ValidatorFn | ValidatorFn[]	
    + 返回值：void
</br>

+ **setAsyncValidators()**
    + 设置该控件上所激活的异步验证器。调用它就会覆盖所有现存的异步验证器。
    + setAsyncValidators(newValidator: AsyncValidatorFn | AsyncValidatorFn[]): void
    + 参数：newValidator	AsyncValidatorFn | AsyncValidatorFn[]	   
    + 返回值：void
</br>

+ **clearValidators()**
    + 清空同步验证器列表
    + 无参数和返回值
</br>

+ **clearAsyncValidators()**
    + 清空异步验证器列表
    + 无参数和返回值
</br>

+ **markAsTouched()**
    + 把该控件标记为 touched。控件获得焦点并失去焦点不会修改这个值。类似的还有markAsUntouched()、markAsDirty()、markAsPristine()
    + markAsTouched(opts: { onlySelf?: boolean; } = {}): void
    + 参数：opts	object	
        + 在应用完此标记后，该配置项会决定控件如何传播变更及发出事件。
        + onlySelf：如果为 true 则只标记当前控件。如果为 false 或不提供，则标记它所有的直系祖先。默认为 false。
        + 可选. 默认值是 {}。
    + 返回值：void
</br>

+ **markAllAsTouched()**
    + 把该控件以及其所有子组件标记为touched。控件获得焦点并失去焦点不会修改这个值。
    + markAllAsTouched(): void
    + 没有参数和返回值
</br>

+ **markAsUntouched()**
    + 把该控件标记为untouched
    + 如果该控件有任何子控件，还会把所有子控件标记为 untouched，并重新计算所有父控件的 touched 状态。
    + markAsUntouched(opts: { onlySelf?: boolean; } = {}): void
    + 参数：opts	object	
        + 在应用完此标记后，该配置项会决定控件如何传播变更及发出事件。
        + onlySelf：如果为 true 则只标记当前控件。如果为 false 或不提供，则标记它所有的直系祖先。默认false。
        + 可选. 默认值是 {}。
</br>

+ **markAsDirty()**
    + 把控件标记为 dirty。当控件通过 UI 修改过时控件会变成 dirty 的；与 markAsTouched 相对。
    + markAsDirty(opts: { onlySelf?: boolean; } = {}): void
    + 参数：opts	object	
        + 在应用完此标记后，该配置项会决定控件如何传播变更以及发出事件。
        + onlySelf：如果为 true 则只标记当前控件。如果为 false 或不提供，则标记它所有的直系祖先。默认false。
    + 可选. 默认值是 {}.
</br>

+ **markAsPristine()**
    + 把该控件标记为 pristine（原始状态）
    + 如果该控件有任何子控件，则把所有子控件标记为 pristine，并重新计算所有父控件的 pristine 状态。
</br>

+ **markAsPending()**
    + 把该控件标记为 pending（待定）的。
    + markAsPending(opts: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
    + 参数：opts	object	
        + 在应用完此标记后，该配置项会决定控件如何传播变更以及发出事件。
        + onlySelf：如果为 true 则只标记当前控件。如果为 false 或不提供，则标记它所有的直系祖先。默认为 false。
        + emitEvent：如果为 true 或未提供（默认值），则 statusChanges（Observable）会发出一个事件，传入控件的最近状态，并把控件标记为 pending 状态。 如果为 false，则不会发出事件。
        + 可选. 默认值是 {}.
    + 当控件正在执行异步验证时，该控件是 pending 的。
</br>

+ **disable()**
    + 禁用此控件。这意味着该控件在表单验证检查时会被豁免，并且从其父控件的聚合值中排除它的值。它的状态是 DISABLED。
    + disable(opts: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
    + 参数：opts	object	
        + 在该控件被禁用之后，该配置项决定如何传播更改以及发出事件。
        + onlySelf：如果为 true，则只标记当前控件。如果为 false 或没有提供，则标记所有直系祖先。默认为 false。
        + emitEvent：如果为 true 或没有提供（默认），则当控件被禁用时，statusChanges 和 valueChanges 这两个 Observable 都会发出最近的状态和值。 如果为 false，则不会发出事件。
        + 可选. 默认值是 {}.
    + 如果该控件有子控件，则所有子控件也会被禁用。
</br>

+ **enable()**
    + 启用该控件。这意味着该控件包含在有效性检查中，并会出现在其父控件的聚合值中。它的状态会根据它的值和验证器而重新计算。
    + 默认情况下，如果该控件具有子控件，则所有子控件都会被启用。
</br>

+ **setParent()**
    + setParent(parent: FormGroup | FormArray): void
    + 参数parent：FormGroup | FormArray	
        + 设置该控件的父控件
    + 返回值：void
</br>

+ **setValue()**
    + 设置该控件的值。这是一个抽象方法（由子类实现）
    + abstract setValue(value: any, options?: Object): void
        + 参数：value	any	；options	Object 可选. 默认值是 undefined.
</br>

+ **patchValue()**
    + 修补（patch）该控件的值。这是一个抽象方法（由子类实现）。
    + abstract patchValue(value: any, options?: Object): void
</br>

+ **reset()**
    + 重置控件。这是一个抽象方法（由子类实现）。
    + abstract reset(value?: any, options?: Object): void
</br>

+ **updateValueAndValidity()**
    + 重新计算控件的值和校验状态。
    + updateValueAndValidity(opts: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
    + 默认情况下，它还会更新其直系祖先的值和有效性状态。
</br>

+ **setErrors()**
    + 在手动（而不是自动）运行校验之后，设置表单控件上的错误信息。
    + setErrors(errors: ValidationErrors, opts: { emitEvent?: boolean; } = {}): void
    + 参数
        + errors	ValidationErrors	
        + opts	object:可选. 默认值是 {}.
    + 调用 setErrors 还会更新父控件的有效性状态。
</br>

+ **get()**
    + 根据指定的控件名称或路径获取子控件。
    + get(path: string | (string | number)[]): AbstractControl | null
    + 参数：
        + path	string | (string | number)[]：一个由点号（.）分隔的字符串或 "字符串/数字" 数组定义的控件路径。
    + 返回值：AbstractControl | null
</br>

+ **getError()**
    + 报告具有指定路径的控件的错误数据。
    + getError(errorCode: string, path?: string | (string | number)[]): any
    + 参数
        + errorCode	string：所查出的错误的错误码
        + path	string | (string | number)[]：一个控件名列表，用于指定要如何从当前控件移动到要查询错误的那个控件。可选. 默认值是 undefined.
    + 返回值：特定错误的数据，如果该控件不存在或没有错误，则返回 null。
</br>

+ **hasError()**
    + 报告指定路径下的控件上是否有指定的错误。
    + hasError(errorCode: string, path?: string | (string | number)[]): boolean
    + 参数
        + errorCode	string：要获取的数据的错误码
        + path	string | (string | number)[]：可选. 默认值是 undefined.
    + 返回值：boolean：如果指定路径下的控件有这个错误则返回 true，否则返回 false。
    + 要检查的控件的路径。如果没有提供该参数，则检查该控件中的错误。
</br>

## FormControl

+ 它和 FormGroup 和 FormArray 是 Angular 表单的三大基本构造块之一。 它扩展了 AbstractControl 类，并实现了关于访问值、验证状态、用户交互和事件的大部分基本功能。 


### 构造函数

```javascript
class FormControl extends AbstractControl {
    constructor(
        formState: any = null, 
        validatorOrOpts?: ValidatorFn | AbstractControlOptions | ValidatorFn[],
        asyncValidator?: AsyncValidatorFn | AsyncValidatorFn[]
    )
  ......
}
```

+ 参数
    + formState any:使用一个初始值或定义了初始值和禁用状态的对象初始化该控件。可选. 默认值是 null.
    + validatorOrOpts	ValidatorFn | AbstractControlOptions | ValidatorFn[]
        + 一个同步验证器函数或其数组，或者一个包含验证函数和验证触发器的 AbstractControlOptions 对象。
        + 可选. 默认值是 undefined.
    +  asyncValidator	AsyncValidatorFn | AsyncValidatorFn[]	
        + 一个异步验证器函数或其数组。
        + 可选. 默认值是 undefined.


### 自有方法（5个）

+ **setValue()**
    + 设置该表单控件的新值。
    + setValue(value: any, options: { onlySelf?: boolean; emitEvent?: boolean; emitModelToViewChange?: boolean; emitViewToModelChange?: boolean; } = {}): void
    + 参数
        + value	any：控件的新值。
        + options	object：
            + 当值发生变化时，该配置项决定如何传播变更以及发出事件。 该配置项会传递给 updateValueAndValidity 方法。
            + onlySelf：如果为 true，则每次变更只影响该控件本身，不影响其父控件。默认为 false。
            + emitEvent：如果为 true 或未提供（默认），则当控件值变化时， statusChanges 和 valueChanges 这两个 Observable 都会以最近的状态和值发出事件。 如果为 false，则不会发出事件。
            + emitModelToViewChange：如果为 true 或未提供（默认），则每次变化都会触发一个 onChange 事件以更新视图。
            + emitViewToModelChange：如果为 true 或未提供（默认），则每次变化都会触发一个 ngModelChange 事件以更新模型。
            + 可选. 默认值是 {}.

+ **patchValue()**
    + 修补控件的值。
    + patchValue(value: any, options: { onlySelf?: boolean; emitEvent?: boolean; emitModelToViewChange?: boolean; emitViewToModelChange?: boolean; } = {}): void
    + 在 FormControl 这个层次上，该函数的功能和 setValue 完全相同。 但 FormGroup 和 FormArray 上的 patchValue 则具有不同的行为。

+ **reset()**
    + 重置该表单控件，把它标记为 pristine 和 untouched，并把它的值设置为 null。
    + reset(formState: any = null, options: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
    + 参数
        + formState	any：使用初始值或一个包含初始值和禁用状态的对象来重置该控件。可选. 默认值是 null.
        + options object：当值发生变化时，该配置项会决定控件如何传播变更以及发出事件。
            + onlySelf：如果为 true ，则每个变更只会影响当前控件而不会影响父控件。默认为 false。
            + emitEvent：如果为 true 或未提供（默认），则当控件被重置时， statusChanges 和 valueChanges 这两个 Observable 都会以最近的状态和值发出事件。 如果为 false，则不会发出事件。
            + 可选. 默认值是 {}

+ **registerOnChange()**
    + 注册变更事件的监听器。
    + registerOnChange(fn: Function): void
    + 参数：fn	Function：当值变化时，就会调用该方法。


+ **registerOnDisabledChange()**
    + 注册禁用事件的监听器。
    + registerOnDisabledChange(fn: (isDisabled: boolean) => void): void
    + 参数：fn(isDisabled: boolean) => void：当禁用状态发生变化时，就会调用该方法。


## FormGroup

跟踪一组 FormControl 实例的值和有效性状态。

+ FormGroup 把每个子 FormControl 的值聚合进一个对象，它的 key 是每个控件的名字。 它通过归集其子控件的状态值来计算出自己的状态。 比如，如果组中的任何一个控件是无效的，那么整个组就是无效的。

+ FormGroup 是 Angular 中用来定义表单的三大基本构造块之一，就像 FormControl、FormArray 一样。

+ 当实例化 FormGroup 时，在第一个参数中传入一组子控件。每个子控件会用控件名把自己注册进去。

### 构造函数

```javascript
    constructor(
        controls: { [key: string]: AbstractControl; }, 
        validatorOrOpts?: ValidatorFn | AbstractControlOptions | ValidatorFn[], 
        asyncValidator?: AsyncValidatorFn | AsyncValidatorFn[]
    )
```

+ 参数
    + controls：object	
        + 一组子控件。每个子控件的名字就是它注册时用的 key。
    + validatorOrOpts	ValidatorFn | AbstractControlOptions | ValidatorFn[]	
        + 一个同步验证器函数或其数组，或者一个包含验证函数和验证触发器的 AbstractControlOptions 对象。
        + 可选. 默认值是 undefined.
    + asyncValidator	AsyncValidatorFn | AsyncValidatorFn[]	
        + 单个的异步验证器函数或其数组。
        + 可选. 默认值是 undefined.

 
### 自有属性（1个）

+ controls: { [key: string]: AbstractControl; }	
    + Declared in constructor.
    + 一组子控件。每个子控件的名字就是它注册时用的 key。

### 自有方法（9个）

+ **registerControl()**
    + 向组内的控件列表中注册一个控件。
    + registerControl(name: string, control: AbstractControl): AbstractControl
    + 参数
        + name string：注册到集合中的控件名
        + control AbstractControl：提供这个名字对应的控件
    + 返回值：AbstractControl
    + 该方法不会更新控件的值或其有效性。 使用 addControl 代替。

+ **addControl()**
    + 往组中添加一个控件。
    + addControl(name: string, control: AbstractControl): void
    + 参数
        + name string：要注册到集合中的控件名
        + control AbstractControl：提供与该控件名对应的控件。
    + 返回值：void
    + 该方法还会更新本控件的值和有效性。

+ **removeControl()**
    + 从该组中移除一个控件。
    + removeControl(name: string): void
    + 参数
        + name string：要从集合中移除的控件名
    + 返回值：void

+ **setControl()**
    + 替换现有控件。
    + setControl(name: string, control: AbstractControl): void
    + 参数
        + name string：要从集合中替换掉的控件名
        + control AbstractControl：提供具有指定名称的控件
    + 返回值：void

+ **contains()**
    + 检查组内是否有一个具有指定名字的`已启用`的控件
    + contains(controlName: string): boolean
    + 参数：controlName	string
    + 返回值：对于已禁用的控件返回 false，否则返回 true。
    + 对于已禁用的控件，返回 false。如果你只想检查它是否存在于该组中，请改用 get 代替。

+ **setValue()**
    + 设置此 FormGroup 的值。它接受一个与组结构对应的对象，以控件名作为 key。
    + setValue(value: { [key: string]: any; }, options: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
    + 参数
        + value	object：控件的新值，其结构必须和该组的结构相匹配。
        + options	object	
            + 当值变化时，此配置项会决定该控件会如何传播变更以及发出事件。该配置项会被传给 updateValueAndValidity 方法。
            + onlySelf:：如果为 true，则每个变更仅仅影响当前控件，而不会影响父控件。默认为 false。
            + emitEvent：如果为 true 或未提供（默认），则当控件值发生变化时，statusChanges 和 valueChanges 这两个 Observable 分别会以最近的状态和值发出事件。 如果为 false 则不发出事件。
            + 可选. 默认值是 {}。
    + 返回值：void

+ **patchValue()**
    + 修补此 FormGroup 的值。它接受一个以控件名为 key 的对象，并尽量把它们的值匹配到组中正确的控件上。
    + patchValue(value: { [key: string]: any; }, options: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
    + 参数
        + value	object：与该组的结构匹配的对象。
        + options	object	
            + 在修补了该值之后，此配置项会决定控件如何传播变更以及发出事件。
            + onlySelf:如果为 true，则每个变更仅仅影响当前控件，而不会影响父控件。默认为 false。
            + emitEvent：如果为 true 或未提供（默认），则当控件值发生变化时，statusChanges 和 valueChanges 这两个 Observable 分别会以最近的状态和值发出事件。 如果为 false 则不发出事件。 该配置项会被传给 updateValueAndValidity 方法。
            + 可选. 默认值是 {}.
    + 返回值：void
    + 它能接受组的超集和子集，而不会抛出错误。

+ **reset()**
    + 重置这个 FormGroup，把它的各级子控件都标记为 `pristine` 和 `untouched`，并把它们的值都设置为 null。
    + reset(value: any = {}, options: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
    + 参数
        + value	any：可选. 默认值是 {}.
        + options	object	
            + 当该组被重置时，此配置项会决定该控件如何传播变更以及发出事件。
            + onlySelf:：如果为 true，则每个变更仅仅影响当前控件，而不会影响父控件。默认为 false。
            + emitEvent：如果为 true 或未提供（默认），则当控件值发生变化时，statusChanges 和 valueChanges 这两个 Observable 分别会以最近的状态和值发出事件。 如果为 false 则不发出事件。 该配置项会被传给 updateValueAndValidity 方法。
            + 可选. 默认值是 {}.
    + 返回值：void
    + 你可以通过传入一个与表单结构相匹配的以控件名为 key 的 Map，来把表单重置为特定的状态。 其状态可以是一个单独的值，也可以是一个同时具有值和禁用状态的表单状态对象。

+ **getRawValue()**
    + 这个 FormGroup 的聚合值，包括所有已禁用的控件。
    + getRawValue(): any
    + 参数：无，返回值：any
    + 获取所有控件的值而不管其禁用状态。 value 属性是获取组中的值的最佳方式，因为它从 FormGroup 中排除了所有已禁用的控件。



## FormArray

跟踪一个控件数组的值和有效性状态，控件可以是 FormControl、FormGroup 或 FormArray 的实例。

+ FormArray 聚合了数组中每个表单控件的值。 它还会根据其所有子控件的状态总结出自己的状态。比如，如果 FromArray 中的任何一个控件是无效的，那么整个数组也会变成无效的。
+ FormArray 是 Angular 表单中定义的三个基本构造块之一，就像 FormControl 和 FormGroup 一样。

### 构造函数

```javascript
constructor(
    controls: AbstractControl[], 
    validatorOrOpts?: ValidatorFn | AbstractControlOptions | ValidatorFn[], 
    asyncValidator?: AsyncValidatorFn | AsyncValidatorFn[]
)
```

+ controls AbstractControl[]: 一个子控件数组。在注册后，每个子控件都会有一个指定的索引。
+ validatorOrOpts ValidatorFn | AbstractControlOptions | ValidatorFn[]	
    + 一个同步验证器函数或其数组，或者一个包含验证函数和验证触发器的 AbstractControlOptions 对象。
    + 可选. 默认值是 undefined.
+ asyncValidator AsyncValidatorFn | AsyncValidatorFn[]	
    + 单个的异步验证器函数或其数组。
    + 可选. 默认值是 undefined.

### 自有的属性（2个）

+ controls: AbstractControl[]
    + Declared in constructor.
    + 一个子控件数组。在注册后，每个子控件都会有一个指定的索引。

+ length: number：只读，控件数组的长度。

### 自有方法（9个）

+ **at()**
    + 获取数组中指定 index 处的 AbstractControl。
    + at(index: number): AbstractControl
    
+ **push()**
    + 在数组的末尾插入一个新的 AbstractControl。
    + push(control: AbstractControl): void

+ **insert()**
    + 在数组中的指定 index 处插入一个新的 AbstractControl。
    + insert(index: number, control: AbstractControl): void

+ **removeAt()**
    + 移除位于数组中的指定 index 处的控件。
    + removeAt(index: number): void

+ **setControl()**
    + 替换现有控件
    + setControl(index: number, control: AbstractControl): void

+ **setValue()**
    + 设置此 FormArray 的值。它接受一个与控件结构相匹配的数组。
    + setValue(value: any[], options: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
    + 该方法会执行严格检查，如果你视图设置不存在或被排除出去的控件的值，就会抛出错误。

+ **patchValue()**
    + 补此 FormArray 的值。它接受一个与该控件的结构相匹配的数组，并尽量把它们的值匹配到组中正确的控件上。
    + patchValue(value: any[], options: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
    + 它能接受数组的超集和子集，而不会抛出错误。
    
+ **reset()**
    + 重置这个 FormArray，把它的各级子控件都标记为 pristine 和 untouched，并把它们的值都设置为 null。
    + reset(value: any = [], options: { onlySelf?: boolean; emitEvent?: boolean; } = {}): void
    + 参数
        + value	any
            + 各个控件值的数组
            + 可选. 默认值是 [].
        + options	object	
            + 当值变化时，此配置项会决定该控件如何传播变更以及发出事件。
            + onlySelf:：如果为 true，则每个变更仅仅影响当前控件，而不会影响父控件。默认为 false。  
            + emitEvent：如果为 true 或未提供（默认），则当控件值发生变化时，statusChanges 和 valueChanges 这两个 Observable 分别会以最近的状态和值发出事件。 如果为 false 则不发出事件。 该配置项会被传给 updateValueAndValidity 方法。
            + 可选. 默认值是 {}.
    + 你可以通过传入一个与表单结构相匹配的状态数组，来把表单重置为特定的状态。 每个状态可以是一个单独的值，也可以是一个同时具有值和禁用状态的表单状态对象。


```javascript
const arr = new FormArray([
   new FormControl(),
   new FormControl()
]);
arr.reset(['name', 'last name']);

console.log(this.arr.value);  // ['name', 'last name']

this.arr.reset([
  {value: 'name', disabled: true},
  'last'
]);

```

+ **getRawValue()**
    + 这个 FormArray 的聚合值，包括所有已禁用的控件。
    + getRawValue(): any[]
    + 没有参数
    + 获取所有控件的值而不管其禁用状态。 如果只想获取已启用的控件的值，则最好使用 value 属性来获取此数组的值。

### FormGroup 和 FormArray 的区别


### 从表单数组中添加或删除控件

要改变数组中的控件列表，可以使用 FormArray 本身的 push、insert 或 removeAt 方法。这些方法能确保表单数组正确的跟踪这些子控件。 不要直接修改实例化 FormArray 时传入的那个 AbstractControl 数组，否则会导致奇怪的、非预期的行为，比如破坏变更检测机制。


-----

做个勤于思考的人~

