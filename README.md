using System;
using System.Collections.Generic;

public class SupermarketBillingSystem
{
    private static List<Product> products = new List<Product>()
    {
        new Product("Apple", 1.00m),
        new Product("Banana", 0.50m),
        new Product("Milk", 2.50m),
        new Product("Bread", 2.00m),
        new Product("Eggs", 3.00m)
    };

    private static List<Order> orders = new List<Order>();

    public static void Main(string[] args)
    {
        Console.WriteLine("Welcome to the Supermarket Billing System!");

        while (true)
        {
            Console.WriteLine("nChoose an option:");
            Console.WriteLine("1. View Products");
            Console.WriteLine("2. Place Order");
            Console.WriteLine("3. View Orders");
            Console.WriteLine("4. Exit");

            int choice = GetIntInput("Enter your choice: ");

            switch (choice)
            {
                case 1:
                    ViewProducts();
                    break;
                case 2:
                    PlaceOrder();
                    break;
                case 3:
                    ViewOrders();
                    break;
                case 4:
                    Console.WriteLine("Exiting the system. Goodbye!");
                    return;
                default:
                    Console.WriteLine("Invalid choice. Please try again.");
                    break;
            }
        }
    }

    private static void ViewProducts()
    {
        Console.WriteLine("nAvailable Products:");
        if (products.Count == 0)
        {
            Console.WriteLine("No products available.");
        }
        else
        {
            for (int i = 0; i < products.Count; i++)
            {
                Console.WriteLine($"{i + 1}. {products[i].Name} - ${products[i].Price}");
            }
        }
    }

    private static void PlaceOrder()
    {
        Console.WriteLine("nPlacing Order");

        Console.Write("Enter customer name: ");
        string customerName = Console.ReadLine();

        List<OrderItem> orderItems = new List<OrderItem>();
        while (true)
        {
            ViewProducts();
            Console.Write("Enter product number to add (or 0 to finish): ");
            int productNumber = GetIntInput("");

            if (productNumber == 0)
            {
                break;
            }
            else if (productNumber > 0 && productNumber <= products.Count)
            {



                Console.Write("Enter quantity: ");
                int quantity = GetIntInput("");
                orderItems.Add(new OrderItem(products[productNumber - 1], quantity));
            }
            else
            {
                Console.WriteLine("Invalid product number. Please try again.");
            }
        }

        if (orderItems.Count > 0)
        {
            Order newOrder = new Order(customerName, orderItems);
            orders.Add(newOrder);
            Console.WriteLine("Order placed successfully!");
            Console.WriteLine($"Order ID: {newOrder.Id}");
            DisplayOrderSummary(newOrder);
        }
        else
        {
            Console.WriteLine("No items added to the order. Order not placed.");
        }
    }

    private static void DisplayOrderSummary(Order order)
    {
        Console.WriteLine("nOrder Summary:");
        Console.WriteLine($"Customer: {order.CustomerName}");
        Console.WriteLine($"Items:");
        foreach (OrderItem item in order.Items)
        {
            Console.WriteLine($"- {item.Product.Name} x{item.Quantity} - ${item.Product.Price * item.Quantity:F2}");
        }
        Console.WriteLine($"Total: ${order.CalculateTotal():F2}");
    }

    private static void ViewOrders()
    {
        Console.WriteLine("nPlaced Orders:");
        if (orders.Count == 0)
        {
            Console.WriteLine("No orders placed yet.");
        }
        else
        {
            foreach (Order order in orders)
            {
                DisplayOrderSummary(order);
                Console.WriteLine("---------------------");
            }
        }
    }

    // Helper method to get integer input from the user
    private static int GetIntInput(string prompt)
    {
        while (true)
        {
            Console.Write(prompt);
            if (int.TryParse(Console.ReadLine(), out int input))
            {
                return input;
            }
            Console.WriteLine("Invalid input. Please enter a number.");
        }
    }
}

// Product class
public class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }

    public Product(string name, decimal price)
    {
        Name = name;
        Price = price;
    }
}

// OrderItem class
public class OrderItem
{
    public Product Product { get; set; }
    public int Quantity { get; set; }

    public OrderItem(Product product, int quantity)
    {
        Product = product;
        Quantity = quantity;
    }
}

// Order class
public class Order
{
    private static int nextOrderId = 1;

    public int Id { get; private set; }
    public string CustomerName { get; private set; }
    public List<OrderItem> Items { get; private set; }

    public Order(string customerName, List<OrderItem> items)
    {
        Id = nextOrderId++;
        CustomerName = customerName;
        Items = items;
    }

    public decimal CalculateTotal()
    {
        decimal total = 0;
        foreach (OrderItem item in Items)
        {
            total += item.Product.Price * item.Quantity;
        }
        return total;
    }
}
