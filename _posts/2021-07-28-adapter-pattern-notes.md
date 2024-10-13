---
title: Adapter Pattern Notes
date: 2021-07-28 09:00:00 +1000
categories: [Programming, Design Patterns]
tags: [object-oriented programming, design patterns, java]
---

**_converts the interface of a class into another interface that a client wants_**

provide the `interface` **according to client requirement** while using the services of a class with a different interface

- Benefits
  - It allows >=2 **previously incompatible** `objects` to interact
  - It allows **reusability of existing functionality**
- Usage
  - When an object needs to **utilise an existing class** with an **incompatible** interface
  - When you want to create a **reusable class** that cooperates with classes which **don't have compatible** `interfaces`

## Implementation 1

1. Create a `CreditCard`(Target) interface

   ```java
   public interface CreditCard {
       public void giveBankDetails();
       public String getCreditCard();
   }
   ```

2. Create a `BankDetails`(Adaptee) class

   ```java
   public class BankDetails{
       private String bankName;
       private String accHolderName;
       private long accNumber;

       public String getBankName() { return bankName; }
       public void setBankName(String bankName) { this.bankName = bankName; }
       public String getAccHolderName() { return accHolderName; }
       public void setAccHolderName(String accHolderName) { this.accHolderName = accHolderName; }
       public long getAccNumber() { return accNumber; }
       public void setAccNumber(long accNumber) { this.accNumber = accNumber; }
   }
   ```

3. Create a `BankCustomer`(Adapter) class that extend `BankDetails` and implement `CreditCard`

   ```java
   import java.io.BufferedReader;
   import java.io.InputStreamReader;
   public class BankCustomer extends BankDetails implements CreditCard {
       public void giveBankDetails() {
           try {
               BufferedReader br=new BufferedReader(new InputStreamReader(System.in));

               System.out.print("Enter the account holder name :");
               String customername=br.readLine();
               System.out.print("\n");

               System.out.print("Enter the account number:");
               long accno=Long.parseLong(br.readLine());
               System.out.print("\n");

               System.out.print("Enter the bank name :");
               String bankname=br.readLine();

               setAccHolderName(customername);
               setAccNumber(accno);
               setBankName(bankname);
           } catch(Exception e) {
               e.printStackTrace();
           }
       }

       @Override
       public String getCreditCard() {
           long accno = getAccNumber();
           String accholdername = getAccHolderName();
           String bname = getBankName();

           return ("The Account number "+accno+" of "+accholdername+" in "+bname+ "
                   bank is valid and authenticated for issuing the credit card. ");
       }
   }
   ```

4. Create a `AdapterPatternDemo`(client) class
   ```java
   public class AdapterPatternDemo {
       public static void main(String args[]){
           CreditCard targetInterface=new BankCustomer();
           targetInterface.giveBankDetails();
           System.out.print(targetInterface.getCreditCard());
       }
   }
   ```

## Implementation 2

1. Create `Target` interface

   ```java
   public interface LightningPhone {
       public void recharge();
       public void useLightning();
   }
   ```

   ```java
   public interface MicroUsbPhone {
       public void recharge();
       public void useMicroUsb();
   }
   ```

2. Create `Adaptee` class that implement `Target` interface

   ```java
   public class Iphone implements LightningPhone {
       private boolean connector;

       @Override
       public void useLightning() {
           connector = true;
           System.out.println("Lightning connected");
       }

       @Override
       public void recharge() {
           if (connector) {
               System.out.println("Recharge started");
               System.out.println("Recharge finished");
           } else {
               System.out.println("Connect Lightning first");
           }
       }
   }
   ```

   ```java
   public class Android implements MicroUsbPhone {
       private boolean connector;

       @Override
       public void useMicroUsb() {
           connector = true;
           System.out.println("MicroUsb connected");
       }

       @Override
       public void recharge() {
           if (connector) {
               System.out.println("Recharge started");
               System.out.println("Recharge finished");
           } else {
               System.out.println("Connect MicroUsb first");
           }
       }
   }
   ```

3. Create `Adapter` class that implement `Target` interface

   ```java
   public class LightningToMicroUsbAdapter implements MicroUsbPhone {
       private final LightningPhone lightningPhone;

       public LightningToMicroUsbAdapter(LightningPhone lightningPhone) {
           this.lightningPhone = lightningPhone;
       }

       @Override
       public void useMicroUsb() {
           System.out.println("MicroUsb connected");
           lightningPhone.useLightning();
       }

       @Override
       public void recharge() {
           lightningPhone.recharge();
       }
   }
   ```

4. Create `Client` class

   ```java
   public class AdapterDemo {
       static void rechargeLightningPhone() {
           phone.useLightning();
           phone.recharge();
       }

       static void rechargeMicroUsbPhone() {
           phone.useMicroUsb();
           phone.recharge();
       }

       public static void main(String[] args) {
           Android android = new Android();
           Iphone iphone = new Iphone();

           System.out.println("Recharging android with MicroUsb");
           rechargeMicroUsbPhone(android);
           System.out.println("Recharging iphone with Lightning");
           rechargeLightningPhone(iphone);
           System.out.println("Recharging iphone with MicroUsb");
           rechargeMicroUsbPhone(new LightningToMicroUsbAdapter(iphone));
       }
   }
   ```

## Notes

- "Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces." [GoF]
- The adapter pattern allows the `interface` of an **existing class** to be used as another `interface`, **suitable for a client class**
- The adapter pattern is often used to make **existing classes**(APIs) work **with a client** class **without modifying** their source code
- The **adapter class** maps/joins functionality of two different types/interfaces
- The adapter patter offers a wrapper around an existing useful class, such that a client class can use functionality of the existing class.
- The adapter pattern do **not offer additional functionality**
- The adapter contains an instance of the class it wraps.
  - In this situation, the adapter makes calls to the instance of the wrapped object.
