 //name muhammad zubair    cu:- 4200-2023
 //project name:- banking manangment system.



#include <iostream>
#include <string>
using namespace std;

class Account {
public:
    Account(string accNumber, string accPin, double initialBalance)
        : accountNumber(accNumber), pin(accPin), balance(initialBalance) {}

    bool authenticate(string accPin) {
        return pin == accPin;
    }

    double deposit(double amount) {
        balance += amount;
        return balance;
    }

    string withdraw(double amount) {
        if (amount > balance) {
            return "Insufficient funds";
        }
        balance -= amount;
        return "Withdrawal successful, new balance: " + to_string(balance);
    }

    double getBalance() {
        return balance;
    }

    string transfer(double amount, Account &targetAccount) {
        if (amount > balance) {
            return "Insufficient funds";
        }
        balance -= amount;
        targetAccount.deposit(amount);
        return "Transfer successful, new balance: " + to_string(balance);
    }

private:
    string accountNumber;
    string pin;
    double balance;
};
class ATM {
public:
    ATM() {}

    string createAccount(string accountNumber, string pin, double balance) {
        if (accounts.find(accountNumber) != accounts.end()) {
            return "Account already exists";
        }
        accounts[accountNumber] = Account(accountNumber, pin, balance);
        return "Account created successfully";
    }

    Account* authenticate(string accountNumber, string pin) {
        if (accounts.find(accountNumber) != accounts.end() &&
            accounts[accountNumber].authenticate(pin)) {
            return &accounts[accountNumber];
        }
        return nullptr;
    }

    double deposit(Account* account, double amount) {
        return account->deposit(amount);
    }

    string withdraw(Account* account, double amount) {
        return account->withdraw(amount);
    }

    double checkBalance(Account* account) {
        return account->getBalance();
    }

    string transfer(Account* account, string targetAccountNumber, double amount) {
        if (accounts.find(targetAccountNumber) == accounts.end()) {
            return "Target account not found";
        }
        return account->transfer(amount, accounts[targetAccountNumber]);
    }

    string miniStatement(Account* account) {
        return "Account Balance: " + to_string(account->getBalance());
    }

private:
    unordered_map<string, Account> accounts;
};
int main() {
    ATM atm;

    // Creating some accounts for testing
    atm.createAccount("12345", "1111", 1000);
    atm.createAccount("67890", "2222", 500);

    while (true) {
        cout << "\nWelcome to the ATM" << endl;
        cout << "Enter your account number: ";
        string accountNumber;
        cin >> accountNumber;
        cout << "Enter your PIN: ";
        string pin;
        cin >> pin;

        Account* account = atm.authenticate(accountNumber, pin);
        if (!account) {
            cout << "Invalid account number or PIN" << endl;
            continue;
        }

        while (true) {
            cout << "\n1. Deposit" << endl;
            cout << "2. Withdraw" << endl;
            cout << "3. Check Balance" << endl;
            cout << "4. Transfer" << endl;
            cout << "5. Mini Statement" << endl;
            cout << "6. Exit" << endl;
            cout << "Choose an option: ";
            int choice;
            cin >> choice;

            if (choice == 1) {
                cout << "Enter amount to deposit: ";
                double amount;
                cin >> amount;
                cout << "New balance: " << atm.deposit(account, amount) << endl;
            } else if (choice == 2) {
                cout << "Enter amount to withdraw: ";
                double amount;
                cin >> amount;
                cout << atm.withdraw(account, amount) << endl;
            } else if (choice == 3) {
                cout << "Balance: " << atm.checkBalance(account) << endl;
            } else if (choice == 4) {
                cout << "Enter target account number: ";
                string targetAccountNumber;
                cin >> targetAccountNumber;
                cout << "Enter amount to transfer: ";
                double amount;
                cin >> amount;
                cout << atm.transfer(account, targetAccountNumber, amount) << endl;
            } else if (choice == 5) {
                cout << atm.miniStatement(account) << endl;
            } else if (choice == 6) {
                cout << "Thank you for using the ATM" << endl;
                break;
            } else {
                cout << "Invalid option, please try again" << endl;
            }
        }
    }

    return 0;
}<!---
Zubairr72827377/Zubairr72827377 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
