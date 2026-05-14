# campus-food-order-system
interface Notification {

    void send(String studentName, String foodName);
}

class EmailNotification implements Notification {

    @Override
    public void send(String studentName, String foodName) {

        AppConfig config = AppConfig.getInstance();

        System.out.println("Sending EMAIL notification...");

        System.out.println("Dear " + studentName +
                ", your order for " + foodName +
                " has been received by " +
                config.getUniversityName() + ".");
    }
}

class SmsNotification implements Notification {

    @Override
    public void send(String studentName, String foodName) {

        System.out.println("Sending SMS notification...");

        System.out.println("Hi " + studentName +
                ", your campus food order is confirmed.");
    }
}

class PushNotification implements Notification {

    @Override
    public void send(String studentName, String foodName) {

        System.out.println("Sending PUSH notification...");

        System.out.println("New notification: Your " +
                foodName + " order is being prepared.");
    }
}

class NotificationFactory {

    public static Notification createNotification(
            String notificationType) {

        if(notificationType.equalsIgnoreCase("EMAIL")) {

            return new EmailNotification();
        }

        else if(notificationType.equalsIgnoreCase("SMS")) {

            return new SmsNotification();
        }

        else if(notificationType.equalsIgnoreCase("PUSH")) {

            return new PushNotification();
        }

        throw new IllegalArgumentException(
                "Unknown notification type.");
    }
}

class AppConfig {

    private static AppConfig instance =
            new AppConfig();

    private String universityName =
            "Istanbul Aydin University";

    private double deliveryFee = 25.0;

    private String systemVersion = "v1.0";

    private AppConfig() {
    }

    public static AppConfig getInstance() {

        return instance;
    }

    public String getUniversityName() {

        return universityName;
    }

    public double getDeliveryFee() {

        return deliveryFee;
    }

    public String getSystemVersion() {

        return systemVersion;
    }
}

public class Main {

    public void placeOrder(String studentName,
                           String foodName,
                           String notificationType) {

        AppConfig config = AppConfig.getInstance();

        System.out.println("Order created for: "
                + studentName);

        System.out.println("Food: " + foodName);

        System.out.println("Delivery fee: "
                + config.getDeliveryFee() + " TL");

        System.out.println("System version: "
                + config.getSystemVersion());

        Notification notification =
                NotificationFactory
                .createNotification(notificationType);

        notification.send(studentName, foodName);

        System.out.println("--------------------------------");
    }

    public static void main(String[] args) {

        Main service = new Main();

        service.placeOrder("Ali",
                "Chicken Sandwich",
                "EMAIL");

        service.placeOrder("Zeynep",
                "Vegetarian Pizza",
                "SMS");

        service.placeOrder("Omar",
                "Coffee",
                "PUSH");
    }
}
