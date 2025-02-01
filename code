import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;

import java.sql.*;

public class SpamDetectionSystem extends Application {
    private static final String DB_URL = "jdbc:mysql://localhost:3306/spam_detection";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "password";

    private Connection connection;
    private Statement statement;

    private TextField messageField;
    private Label resultLabel;

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        try {
            establishDatabaseConnection();

            primaryStage.setTitle("Spam Detection System");

            GridPane gridPane = createFormPane();
            addUIControls(gridPane);

            Scene scene = new Scene(gridPane, 400, 200);
            primaryStage.setScene(scene);
            primaryStage.show();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private void establishDatabaseConnection() throws SQLException {
        connection = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);
        statement = connection.createStatement();
    }

    private GridPane createFormPane() {
        GridPane gridPane = new GridPane();
        gridPane.setPadding(new Insets(20, 40, 20, 40));
        gridPane.setHgap(10);
        gridPane.setVgap(10);
        return gridPane;
    }

    private void addUIControls(GridPane gridPane) {
        Label headerLabel = new Label("Spam Detection System");
        headerLabel.setStyle("-fx-font-size: 20px; -fx-font-weight: bold;");
        gridPane.add(headerLabel, 0, 0, 2, 1);

        Label messageLabel = new Label("Message:");
        gridPane.add(messageLabel, 0, 1);

        messageField = new TextField();
        gridPane.add(messageField, 1, 1);

        Button detectButton = new Button("Detect");
        detectButton.setOnAction(e -> detectSpam());
        gridPane.add(detectButton, 0, 2);

        resultLabel = new Label();
        gridPane.add(resultLabel, 0, 3, 2, 1);
    }

    private void detectSpam() {
        String message = messageField.getText();

        try {
            String sql = "SELECT is_spam FROM messages WHERE message = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setString(1, message);
            ResultSet resultSet = preparedStatement.executeQuery();

            if (resultSet.next()) {
                boolean isSpam = resultSet.getBoolean("is_spam");
                if (isSpam) {
                    resultLabel.setText("SPAM detected!");
                } else {
                    resultLabel.setText("Not spam.");
                }
            } else {
                resultLabel.setText("Message not found.");
            }

            resultSet.close();
            preparedStatement.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void stop() throws Exception {
        super.stop();
        if (statement != null) {
            statement.close();
        }
        if (connection != null) {
            connection.close();
        }
    }
}
