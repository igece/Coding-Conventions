# C# Coding Conventions

The conventions observed in this document serve the following purposes:

- They create a consistent look to the code, so that readers can focus on content, not layout.
- They enable readers to understand the code more quickly by making assumptions based on previous experience.
- They facilitate copying, changing, and maintaining the code.
- They demonstrate C# best practices.

# Naming conventions

There are several naming conventions to consider when writing C# code.

In the following examples, any of the guidance pertaining to elements marked public is also applicable when working with protected and protected internal elements, all of which are intended to be visible to external callers.

## Pascal case

Use pascal casing ("PascalCasing") when naming a `class`, `record`, or `struct`.

``` c#
public class DataService
{
}
```
``` c#
public struct ValueCoordinate
{
}
```


When naming an interface, use pascal casing in addition to prefixing the name with an I. This clearly indicates to consumers that it's an interface.

``` c#
public interface IWorkerQueue
{
}
```

When naming public members of types, such as fields, properties, events, methods, and local functions, use pascal casing.

``` c#
public class ExampleEvents
{
    // A public field, these should be used sparingly
    public bool IsValid;

    // An init-only property
    public IWorkerQueue WorkerQueue { get; init; }

    // An event
    public event Action EventProcessing;

    // Method
    public void StartEventProcessing()
    {
        // Local function
        static int CountQueueItems() => WorkerQueue.Count;
        // ...
    }
}
```

## Camel case

Use camel casing ("camelCasing") when naming private or internal fields, and prefix them with _.

``` c#
public class DataService
{
    private IWorkerQueue _workerQueue;
}
```

When writing method parameters, use camel casing.


``` c#
public T SomeMethod<T>(int someNumber, bool isValid)
{
}
```

# Layout conventions

- Good layout uses formatting to emphasize the structure of your code and to make theh code easier to read.

- Use the default Code Editor settings (smart indenting, four-character indents, tabs saved as spaces). For more information, see Options, Text Editor, C#, Formatting.

- Write only one statement per line.

- Write only one declaration per line.

- If continuation lines are not indented automatically, indent them one tab stop (four spaces).

- Add at least one blank line between method definitions and property definitions.

- Use parentheses to make clauses in an expression apparent, as shown in the following code.


``` c#
if ((val1 > val2) && (val1 > val3))
{
    // Take appropriate action.
}
```

# Commenting conventions

- Place the comment on a separate line, not at the end of a line of code.

- Begin comment text with an uppercase letter.

- End comment text with a period.

- Insert one space between the comment delimiter (//) and the comment text, as shown in the following example.

``` c#
// The following declaration creates a query. It does not run
// the query.
```

- Don't create formatted blocks of asterisks around comments.

- Ensure all public members have the necessary XML comments providing appropriate descriptions about their behavior.

# Language guidelines

The following sections describe practices that the C# team follows to prepare code examples and samples.

## String data type

Use string interpolation to concatenate short strings, as shown in the following code.

``` c#
string displayName = $"{nameList[n].LastName}, {nameList[n].FirstName}";
```

To append strings in loops, especially when you're working with large amounts of text, use a StringBuilder object.


``` c#
var phrase = "lalalalalalalalalalalalalalalalalalalalalalalalalalalalalala";
var manyPhrases = new StringBuilder();
for (var i = 0; i < 10000; i++)
{
    manyPhrases.Append(phrase);
}
//Console.WriteLine("tra" + manyPhrases);
```

## Implicitly typed local variables

- Use implicit typing for local variables when the type of the variable is obvious from the right side of the assignment, or when the precise type is not important.

``` c#
var var1 = "This is clearly a string.";
var var2 = 27;
```

- Don't use var when the type is not apparent from the right side of the assignment. Don't assume the type is clear from a method name. A variable type is considered clear if it's a new operator or an explicit cast.

``` c#
int var3 = Convert.ToInt32(Console.ReadLine()); 
int var4 = ExampleClass.ResultSoFar();
```

- Don't rely on the variable name to specify the type of the variable. It might not be correct. In the following example, the variable name inputInt is misleading. It's a string.

``` c#
var inputInt = Console.ReadLine();
Console.WriteLine(inputInt);
```

- Avoid the use of var in place of dynamic. Use dynamic when you want run-time type inference. For more information, see Using type dynamic (C# Programming Guide).

## Unsigned data types

In general, use `int` rather than unsigned types. The use of `int` is common throughout C#, and it is easier to interact with other libraries when you use `int`.

## Arrays

Use the concise syntax when you initialize arrays on the declaration line. In the following example, note that you can't use var instead of string[].

``` c#
string[] vowels1 = { "a", "e", "i", "o", "u" };
```

If you use explicit instantiation, you can use `var`.

``` c#
var vowels2 = new string[] { "a", "e", "i", "o", "u" };
```

If you specify an array size, you have to initialize the elements one at a time.

``` c#
var vowels3 = new string[5];
vowels3[0] = "a";
vowels3[1] = "e";
// And so on.
```

## Delegates

Use `Func<>` and `Action<>` instead of defining delegate types. In a class, define the delegate method.

``` c#
public static Action<string> ActionExample1 = x => Console.WriteLine($"x is: {x}");

public static Action<string, string> ActionExample2 = (x, y) => 
    Console.WriteLine($"x is: {x}, y is {y}");

public static Func<string, int> FuncExample1 = x => Convert.ToInt32(x);

public static Func<int, int, int> FuncExample2 = (x, y) => x + y;
```

Call the method using the signature defined by the `Func<>` or `Action<>` delegate.

``` c#
ActionExample1("string for x");

ActionExample2("string for x", "string for y");

Console.WriteLine($"The value is {FuncExample1("1")}");

Console.WriteLine($"The sum is {FuncExample2(1, 2)}");
```

## `try-catch` and `using` statements in exception handling

Use a try-catch statement for most exception handling.

``` c#
static string GetValueFromArray(string[] array, int index)
{
    try
    {
        return array[index];
    }

    catch (System.IndexOutOfRangeException ex)
    {
        Console.WriteLine("Index is out of range: {0}", index);
        throw;
    }
}
```

Simplify your code by using the C# `using` statement. If you have a `try-finally` statement in which the only code in the finally block is a call to the `Dispose` method, use a using statement instead.

In the following example, the `try-finally` statement only calls `Dispose` in the `finally` block.


``` c#
Font font1 = new Font("Arial", 10.0f);

try
{
    byte charset = font1.GdiCharSet;
}

finally
{
    if (font1 != null)
    {
        ((IDisposable)font1).Dispose();
    }
}
```

You can do the same thing with a using statement.

``` c#
using (Font font2 = new Font("Arial", 10.0f))
{
    byte charset2 = font2.GdiCharSet;
}
```

## `&&` and `||` operators
To avoid exceptions and increase performance by skipping unnecessary comparisons, use `&&` instead of `&` and `||` instead of `|` when you perform comparisons, as shown in the following example.

``` c#
Console.Write("Enter a dividend: ");
int dividend = Convert.ToInt32(Console.ReadLine());

Console.Write("Enter a divisor: ");
int divisor = Convert.ToInt32(Console.ReadLine());

if ((divisor != 0) && (dividend / divisor > 0))
{
    Console.WriteLine("Quotient: {0}", dividend / divisor);
}

else
{
    Console.WriteLine("Attempted division by 0 ends up here.");
}
```

If the divisor is 0, the second clause in the if statement would cause a run-time error. But the `&&` operator short-circuits when the first expression is false. That is, it doesn't evaluate the second expression. The & operator would evaluate both, resulting in a run-time error when divisor is 0.

## new operator
Use one of the concise forms of object instantiation, as shown in the following declarations. The second example shows syntax that is available starting in C# 9.

``` c#
var instance1 = new ExampleClass();
```
``` c#
ExampleClass instance2 = new();
```

The preceding declarations are equivalent to the following declaration.

``` c#
ExampleClass instance2 = new ExampleClass();
```

- Use object initializers to simplify object creation, as shown in the following example.

``` c#
var instance3 = new ExampleClass { Name = "Desktop", ID = 37414,
    Location = "Redmond", Age = 2.3 };
```

The following example sets the same properties as the preceding example but doesn't use initializers.

``` c#
var instance4 = new ExampleClass();
instance4.Name = "Desktop";
instance4.ID = 37414;
instance4.Location = "Redmond";
instance4.Age = 2.3;
```

## Event handling

If you're defining an event handler that you don't need to remove later, use a lambda expression.

``` c#
public Form2()
{
    this.Click += (s, e) =>
        {
            MessageBox.Show(
                ((MouseEventArgs)e).Location.ToString());
        };
}
```

The lambda expression shortens the following traditional definition.

``` c#
public Form1()
{
    this.Click += new EventHandler(Form1_Click);
}

void Form1_Click(object sender, EventArgs e)
{
    MessageBox.Show(((MouseEventArgs)e).Location.ToString());
}
```

## Static members

Call static members by using the class name: ClassName.StaticMember. This practice makes code more readable by making static access clear. Don't qualify a static member defined in a base class with the name of a derived class. While that code compiles, the code readability is misleading, and the code may break in the future if you add a static member with the same name to the derived class.

## LINQ queries

- Use meaningful names for query variables. The following example uses seattleCustomers for customers who are located in Seattle.

``` c#
var seattleCustomers = from customer in customers
                       where customer.City == "Seattle"
                       select customer.Name;
```

- Use aliases to make sure that property names of anonymous types are correctly capitalized, using Pascal casing.

``` c#
var localDistributors =
    from customer in customers
    join distributor in distributors on customer.City equals distributor.City
    select new { Customer = customer, Distributor = distributor };
```

- Rename properties when the property names in the result would be ambiguous. For example, if your query returns a customer name and a distributor ID, instead of leaving them as Name and ID in the result, rename them to clarify that Name is the name of a customer, and ID is the ID of a distributor.

``` c#
var localDistributors2 =
    from customer in customers
    join distributor in distributors on customer.City equals distributor.City
    select new { CustomerName = customer.Name, DistributorID = distributor.ID };
```

- Use implicit typing in the declaration of query variables and range variables.

``` c#
var seattleCustomers = from customer in customers
                       where customer.City == "Seattle"
                       select customer.Name;
```

- Align query clauses under the from clause, as shown in the previous examples.

- Use where clauses before other query clauses to ensure that later query clauses operate on the reduced, filtered set of data.

``` c#
var seattleCustomers2 = from customer in customers
                        where customer.City == "Seattle"
                        orderby customer.Name
                        select customer;
```

- Use multiple from clauses instead of a join clause to access inner collections. For example, a collection of Student objects might each contain a collection of test scores. When the following query is executed, it returns each score that is over 90, along with the last name of the student who received the score.

``` c#
var scoreQuery = from student in students
                 from score in student.Scores
                 where score > 90
                 select new { Last = student.LastName, score };
```
