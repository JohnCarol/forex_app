# forex_app
A forex calculator built using Angularjs, Codeigniter and MySQL

1. Create the action_codes TABLE

        CREATE TABLE `action_codes` (
          `action_id` int(11) NOT NULL,
          `action` varchar(50) NOT NULL
        ) ENGINE=MyISAM DEFAULT CHARSET=latin1;
        INSERT INTO `action_codes` (`action_id`, `action`) VALUES
        (0, 'None'),
        (1, 'Email'),
        (2, 'Discount');
        ALTER TABLE `action_codes`
          ADD PRIMARY KEY (`action_id`);
        ALTER TABLE `action_codes`
          MODIFY `action_id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=4;
          
2. Create the currencies TABLE

        CREATE TABLE `currencies` (
        `id` int(11) NOT NULL,
        `currency_name` varchar(50) NOT NULL,
        `curr_shrt` varchar(5) NOT NULL,
        `rate` decimal(19,7) NOT NULL,
        `surcharge` decimal(10,2) NOT NULL,
        `discount` int(11) NOT NULL DEFAULT '0',
        `last_updated` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
        `action_code_id` int(11) NOT NULL
        ) ENGINE=MyISAM DEFAULT CHARSET=latin1;

        INSERT INTO `currencies` (`id`, `currency_name`, `curr_shrt`, `rate`, `surcharge`, `discount`, `last_updated`, `action_code_id`) VALUES
        (1, 'US Dollar', 'USD', '0.0808279', '7.50', 0, '2016-06-21 09:26:58', 0),
        (2, 'British Pound', 'GBP', '0.0527032', '5.00', 0, '2016-06-21 09:26:58', 1),
        (3, 'EURO', 'EUR', '0.0718710', '5.00', 2, '2016-06-21 09:26:58', 2),
        (4, 'Kenyan Shilling', 'KES', '7.8149800', '2.50', 0, '2016-06-21 09:26:58', 0);
        
        ALTER TABLE `currencies`
          ADD PRIMARY KEY (`id`);
        ALTER TABLE `currencies`
          MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=5;
          
3. Create the orders TABLE

        CREATE TABLE `orders` (
        `id` int(11) NOT NULL,
        `curr` varchar(10) NOT NULL,
        `rate` decimal(19,7) NOT NULL,
        `surcharge` decimal(10,2) NOT NULL,
        `amt_purchased` decimal(15,4) NOT NULL,
        `amt_paid` decimal(15,4) NOT NULL,
        `amt_surcharge` decimal(15,4) NOT NULL,
        `discount` decimal(10,2) NOT NULL DEFAULT '0.00',
        `amt_less_surcharge` decimal(15,4) NOT NULL,
        `order_date` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
        `action_id` int(11) NOT NULL
        ) ENGINE=MyISAM DEFAULT CHARSET=latin1;

        ALTER TABLE `orders`
        ADD PRIMARY KEY (`id`);

        ALTER TABLE `orders`
        MODIFY `id` int(11) NOT NULL AUTO_INCREMENT;
        
4. Create a view for currencies summary(Please set your host and username to match your DB credentials.)
        
      CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY DEFINER VIEW `v_curr_summary`  AS  select `currencies`.`surcharge` AS `surcharge`,`currencies`.`currency_name` AS `currency_name`,`currencies`.`curr_shrt` AS `curr_shrt`,`currencies`.`rate` AS `rate`,`currencies`.`discount` AS `discount`,`currencies`.`action_code_id` AS `action_code_id`,`currencies`.`last_updated` AS `last_updated`,`action_codes`.`action` AS `action` from (`currencies` left join `action_codes` on((`currencies`.`action_code_id` = `action_codes`.`action_id`))) order by `currencies`.`currency_name` ;

5. Create a folder in you htdocs folder called forex-app"
6. Download and Unzip the contents of the forex-app.zip file into the forex-app directory
