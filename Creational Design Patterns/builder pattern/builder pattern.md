## Builder Pattern
### Intent
1. Separate the construction of a complex object from its representation so that you can use the same construction process for different types of objects
2. Parse a complex representation and create one of several target objects out of it

### Definitions
1. Aggregate products - the complex objects composed of two or more child objects

2. Parts - the child objects, and there are lots of possible permutations of parts for making an aggregate product

3. The builder class - the construction process that is responsible for constructing the product by repeatedly creating and adding parts until the product is complete

### Benefits
1. Ideal if he construction process occurs in a number of discrete steps

2. Strict separation between data representation and the construction process

3. Convenient when a single product can be represented by several representations

4. Offers fine-grained control over construction process

### UML
![Builder Pattern UML](img/builder pattern uml.gif)

- The builder class - a base class that specifies an interface for creating the parts of the products
- The director - call the builder abstract methods in the correct order to assemble products
- The concrete builder - a subclass derived from builder that actually constructs and assembles the parts of the products

Steps:
1. Initialize the director with a concrete builder
2. The director provides a construct methods that will call each buildpartsXXX methods to assemble a complete product
3. Product can be retrieved from the builder

### Checklist
1. Make sure you have lots of aggregate products that differ only in their permutations of internal parts, and your construction code involves a number of sequential steps

2. Create a concrete builder class for each product type and move each construction step into a separate build method

3. Design a standard protocol for all build steps and capture the steps of this protocol in the abstract builder class

4. Define a director that calls the builder methods in the correct order

5. Make sure the client does not use new anywhere and only uses the builder to create new instances

### Example
An application for a fast food restaurant

#### Class needed
| Pattern class | Example class |
| --- | --- |
| Builder | MenuBuilder |
|ConcreteBuilder | BurgerMenuBuilder<br/>SaladMenuBuilder<br/>KidsMenuBuilder |
| Director | MenuDirector |
| Product | Menu |

#### C# code
The MenuDirector class
```csharp
namespace FastfoodExample
{
  class MenuDirector
  {
    public void Construct(MenuBuilder builder)
    {
      builder.BuildBurgerOrSalad();
      builder.BuildFries();
      builder.BuildDessert();
      builder.BuildDrink();
      builder.BuildToy();
    }
  }
}
```
The MenuBuilder class
```csharp
namespace FastfoodExample
{
  abstract class MenuBuilder
  {
    public abstract void BuildBurgerOrSalad();
    public abstract void BuildFries();
    public abstract void BuildDessert();
    public abstract void BuildDrink();
    public abstract void BuildToy();

    // Get the finished menu
    public abstract Menu GetResult();
  }
}
```
The concrete builders classes
1. The BurgerMenuBuilder class
```csharp
namespace FastfoodExample
{
  class BurgerMenuBuilder : MenuBuilder
  {
    private Menu _menu = new Menu();

    public override void BuildBurgerOrSalad()
    {
      _menu.Add("Burger");
    }

    public override void BuildFries()
    {
      _menu.Add("Fries");
    }

    public override void BuildDessert()
    {
      _menu.Add("Dessert");
    }

    public override void BuildDrink()
    {
      _menu.Add("Drink");
    }

    public override void BuildToy()
    {
    }
  }
}
```
2. The SaladMenuBuilder class
```csharp
namespace FastfoodExample
{
  class SaladMenuBuilder : MenuBuilder
  {
    private Menu _menu = new Menu();

    public override void BuildBurgerOrSalad()
    {
      _menu.Add("Salad");
    }

    public override void BuildFries()
    {
      _menu.Add("Fries");
    }

    public override void BuildDessert()
    {
      _menu.Add("Dessert");
    }

    public override void BuildDrink()
    {
      _menu.Add("Drink");
    }

    public override void BuildToy()
    {
    }
  }
}
```
3. The KidsMenuBuilder class
```csharp
namespace FastfoodExample
{
  class KidsMenuBuilder : MenuBuilder
  {
    private Menu _menu = new Menu();

    public override void BuildBurgerOrSalad()
    {
      _menu.Add("Burger");
    }

    public override void BuildFries()
    {
      _menu.Add("Fries");
    }

    public override void BuildDessert()
    {
    }

    public override void BuildDrink()
    {
      _menu.Add("Drink");
    }

    public override void BuildToy()
    {
      _menu.Add("Toy");
    }
  }
}
```
The Menu class - the aggregate product
```csharp
namespace FastfoodExample
{
  class Menu
  {
    private Dictionary<string, string> _parts = new Dictionary<string, string>();

    public void Add(string part)
    {
      _parts.Add(part, part);
    }

    public override string ToString()
    {
      return String.Join(", ", _parts.Values);
    }
  }
}
```
The main program
```csharp
namespace FastfoodExample
{
  class MainClass
  {
    public static void Main(string[] args)
    {
      MenuDirector directory = new MenuDirector();

      // Burger menu
      MenuBuilder builder1 = new BurgerMenuBuilder();
      director.Construct(builder1);
      Menu menu1 = builder1.GetResult();
      Console.WriteLine("Burger menu: {0}", menu);

      MenuBuilder builder2 = new SaladMenuBuilder();
      director.Construct(builder2);
      Menu menu1 = builder2.GetResult();
      Console.WriteLine("Salad menu: {0}", menu);

      // Kids menu
      MenuBuilder builder3 = new KidsMenuBuilder();
      director.Construct(builder3);
      Menu menu3 = builder3.GetResult();
      Console.WriteLine("Kids menu: {0}", menu);
    }
  }
}
```
