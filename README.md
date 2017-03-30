# Code
container.RegisterType<IProductServices, ProductServices>()



           var discountsDictionary = new Dictionary<AccountStatus, IAccountDiscountCalculator>
              {
                {AccountStatus.NotRegistered, new NotRegisteredDiscountCalculator()},
                {AccountStatus.SimpleCustomer, new SimpleCustomerDiscountCalculator()},
                {AccountStatus.ValuableCustomer, new ValuableCustomerDiscountCalculator()},
                {AccountStatus.MostValuableCustomer, new MostValuableCustomerDiscountCalculator()}
              };

            var builder = new ContainerBuilder();
            builder.RegisterType<DictionarableAccountDiscountCalculatorFactory>().As<IAccountDiscountCalculatorFactory>()
            .WithParameter("discountsDictionary", discountsDictionary)
            .SingleInstance();
            builder.RegisterType<DefaultLoyaltyDiscountCalculator>().As<ILoyaltyDiscountCalculator>().SingleInstance();
            builder.RegisterType<DiscountManager>().SingleInstance();

            var container = builder.Build();

            using (var scope = container.BeginLifetimeScope())
            {

                /*var myCalculator = new DiscountManager(scope.Resolve<IAccountDiscountCalculatorFactory>(), 
                                                                               scope.Resolve<ILoyaltyDiscountCalculator>());*/
                var myCalculator = scope.Resolve<DiscountManager>();

                listBox1.Items.Add(myCalculator.ApplyDiscount(1, AccountStatus.SimpleCustomer, 10));

            }


            ********builder.RegisterType<TestG<entidad>>().As<ITestG<entidad>>();
