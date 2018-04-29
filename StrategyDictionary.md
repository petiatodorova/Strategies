Dictionary:

1. Отваряме DictionaryVariant1.cs
2. Създаваме си обект с property и методи, каквито ни вършат работа в случая.

-------------

    class Customer
    {
        public string Name { get; set; }
        public string Fruit { get; set; }
        public string Color { get; set; }
        public double Quantity { get; set; }
        public decimal Price { get; set; }

        public decimal TotalMoney
        {
            get
            {
                return (decimal)Quantity * Price;
            }
        }
    }
    
-------------

3. Променяме си каквото трябва в метода за четене на current cutomer - връща ни попълнен customer.

-------------

        static Customer ReadCustomr(string input)
        {
            string[] inputArr = input
                .Split(new char[] { ' ', '/', '\\', ';', ',' }, StringSplitOptions.RemoveEmptyEntries)
                .ToArray();

            return new Customer
            {
                Name = inputArr[0],
                Fruit = inputArr[1],
                Color = inputArr[2],
                Quantity = double.Parse(inputArr[3]),
                Price = decimal.Parse(inputArr[4])
            };
        }
        
        
-------------      

4. Решаваме какъв стринг ни е уникалния ключ, 
- създаваме си SortedDictionary<string, List<Customer>> customers;
- пълним го с while цикъла, докато получим команда за край;
- за всеки     foreach (var cust in customers) си вземаме ключа;
- печатаме го;
- после си вземаме неговия  List<Customer> theValues = cust.Value; 
- обхождаме го със съответните правила и печатаме в съответния вид: 
                foreach (var person in theValues.OrderByDescending(x => x.Name).ThenBy(y => y.TotalMoney))
                {
                    Console.WriteLine($"Name: {person.Name} --> Total Money: {person.TotalMoney}");
                }

- !!! Ако се иска да се сумират негови стойности, 
то при  if (!customers.ContainsKey(color)) си правим нулева стойност за този ключ 
и после си добавяме и в двата случая (натрупваме)
                
-------------

        static void Main(string[] args)
        {
            SortedDictionary<string, List<Customer>> customers = new SortedDictionary<string, List<Customer>>();
            string newInput = Console.ReadLine();

            while (true)
            {
                if (newInput == "End")
                {
                    break;
                }
                Customer currCustomer = ReadCustomr(newInput);
                string color = currCustomer.Color;

                if (!customers.ContainsKey(color))
                {
                    customers[color] = new List<Customer>();
                }
                customers[color].Add(currCustomer);

                newInput = Console.ReadLine();
            }

            foreach (var cust in customers)
            {
                string theColor = cust.Key;
                Console.WriteLine($"The Color: --> {theColor}");
                List<Customer> theValues = cust.Value;
                foreach (var person in theValues.OrderByDescending(x => x.Name).ThenBy(y => y.TotalMoney))
                {
                    Console.WriteLine($"Name: {person.Name} --> Total Money: {person.TotalMoney}");
                }
            }
        }

-------------