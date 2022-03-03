---
id: mpylwaarnttr294ymrcyees
title: Hledger
desc: ''
updated: 1646193033529
created: 1646135339071
---

# Hledger
Based on Ledger-Cli. Coded with Haskell.

## Installation
* Download the latest release: https://github.com/simonmichael/hledger/releases (take the major release version)
* Download the appropriate "hledger-PLATFORM.zip" file below.
* Unzip it to get 2 or 3 hledger binaries in the current directory.
* On GNU/Linux, you will need to chmod +x these files to make them executable. They should run on most GNU/Linux machines with x64 or (when provided) arm32v7 architecture.
* Use with
  ```sh
  ./hledger-linux-static-x64 -f .hledger.journal print
  ```
* Or add to $PATH for global usage
  ```sh
  sudo ln -s /<abs_path>/hledger-linux-static-x64 /usr/local/bin/hledger
  ```

## Translate csv to journal
### File format
1. A journal file
   ```
   ; $HOME/.hledger.journal
   2020-01-01 opening balances
       assets:checking         $1234
       equity
   
   2020-03-15 client payment
       assets:checking         $2000
       income:consulting
   
   2020-03-20 Sprouts
       expenses:food:groceries  $100
       assets:cash               $40
       assets:checking
   ```

2. My csv file exported from Money Manager

   | Date  | Account | Category | SubCategory | Note  |  HKD  | Income/Expense | Note  | Amount | Currency | Account |
   | :---: | :-----: | :------: | :---------: | :---: | :---: | :------------: | :---: | :----: | :------: | :-----: |
   |       |         |          |             |       |       |                |       |        |          |         |
   
   The concerned columns are:
   
   | Date  |    Account    |        Category         |     SubCategory     |    Note     | ~~HKD~~ |            Income/Expense             | Note (additional note) | Amount | Currency | ~~Account~~ |
   | :---: | :-----------: | :---------------------: | :-----------------: | :---------: | :-----: | :-----------------------------------: | :--------------------: | :----: | :------: | :---------: |
   | date  | asset account | expense / asset account | expense subcategory | description |   ---   | condition (income, expense, transfer) |    additional note     | amount | currency |     ---     |

### Conversion logic
* To convert from csv to journal
  1. Specify all assets with groups
     1. e.g. assets:bank:HSBC.
     2. assets:credit card:SimplyCash
     3. assets:digital wallet
  2. For all asset account -> need to match account with the specified assets
  3. If 'Income/Expense' column is `Income`
     1. asset account1 -> account
     2. account2 -> income:<Category\>:<SubCategory\>
     3. amount1 -> **+**amount
     4. amount2 -> -amount
  4. else if 'Income/Expense' column is `Expense`
     1. asset account1 -> account
     2. account2 -> expense:<Category\>:<SubCategory\>
     3. amount1 -> **-**amount
     4. amount2 -> +amount
  5. else if 'Income/Expense' column is `Trasnsfer-Out`
     1. asset account1 -> account
     2. account2 -> asset account -> <Category\>
     3. amount1 -> **-**amount
     4. amount2 -> +amount
  6. currency -> currency
  7. Date -> date
  8. Note -> note
  9. additional note -> additional note

journal format: https://hledger.org/add.html
