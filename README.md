import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;

public class App extends Application {

    private BankAccount bankAccount;

    @Override
    public void start(Stage primaryStage) {
        // Create a bank account with initial balance of 1000
        bankAccount = new BankAccount(1000);

        // Create UI elements
        Label balanceLabel = new Label("Balance:");
        TextField balanceField = new TextField();
        balanceField.setEditable(false);
        balanceField.setText(String.valueOf(bankAccount.getBalance()));

        Label amountLabel = new Label("Amount:");
        TextField amountField = new TextField();

        Button withdrawButton = new Button("Withdraw");
        withdrawButton.setOnAction(e -> {
            double amount = Double.parseDouble(amountField.getText());
            String message = bankAccount.withdraw(amount);
            balanceField.setText(String.valueOf(bankAccount.getBalance()));
            showMessage(message);
        });

        Button depositButton = new Button("Deposit");
        depositButton.setOnAction(e -> {
            double amount = Double.parseDouble(amountField.getText());
            String message = bankAccount.deposit(amount);
            balanceField.setText(String.valueOf(bankAccount.getBalance()));
            showMessage(message);
        });

        // Layout
        GridPane gridPane = new GridPane();
        gridPane.setPadding(new Insets(20));
        gridPane.setHgap(10);
        gridPane.setVgap(10);
        gridPane.add(balanceLabel, 0, 0);
        gridPane.add(balanceField, 1, 0);
        gridPane.add(amountLabel, 0, 1);
        gridPane.add(amountField, 1, 1);
        gridPane.add(withdrawButton, 0, 2);
        gridPane.add(depositButton, 1, 2);

        // Scene
        Scene scene = new Scene(gridPane, 300, 150);

        // Stage
        primaryStage.setTitle("ATM");
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    private void showMessage(String message) {
        // Display a message dialog or toast with the given message
        System.out.println(message);
    }

    public static void main(String[] args) {
        launch(args);
    }
}

class BankAccount {
    private double balance;

    public BankAccount(double balance) {
        this.balance = balance;
    }

    public double getBalance() {
        return balance;
    }

    public String deposit(double amount) {
        balance += amount;
        return "Deposited " + amount + ". Current balance: " + balance;
    }

    public String withdraw(double amount) {
        if (amount > balance) {
            return "Insufficient funds";
        }
        balance -= amount;
        return "Withdrew " + amount + ". Current balance: " + balance;
    }
}
