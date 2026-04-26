public class TestOrder {

    enum OrderStatus {
        PENDING,
        PAYMENT_COMPLETED,
        SHIPPED,
        COMPLETED,
        CANCELLED
    }

    static class Order {
        OrderStatus status;

        public void updateOrderStatus(OrderStatus newStatus) throws Exception {

            if (this.status == OrderStatus.COMPLETED || this.status == OrderStatus.CANCELLED) {
                throw new Exception("Order already finished.");
            }

            if (newStatus == OrderStatus.SHIPPED && this.status != OrderStatus.PAYMENT_COMPLETED) {
                throw new Exception("Cannot ship before payment.");
            }

            if (newStatus == OrderStatus.COMPLETED && this.status != OrderStatus.SHIPPED) {
                throw new Exception("Cannot complete before shipping.");
            }

            this.status = newStatus;
        }
    }

    public static void main(String[] args) {
        try {
            Order order = new Order();

            order.status = OrderStatus.PENDING;
            System.out.println("Initial: " + order.status);

            order.updateOrderStatus(OrderStatus.PAYMENT_COMPLETED);
            System.out.println("After Payment: " + order.status);

            order.updateOrderStatus(OrderStatus.SHIPPED);
            System.out.println("After Shipping: " + order.status);

            order.updateOrderStatus(OrderStatus.COMPLETED);
            System.out.println("Final: " + order.status);

        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}