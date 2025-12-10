import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;
import java.util.List;

public class MovieBookingGUI extends JFrame {
    private JComboBox<String> movieComboBox;
    private JTextField nameField;
    private JTextField ticketCountField;
    private JButton bookButton;
    private JPanel seatPanel;
    private List<JButton> seatButtons;
    private List<String> selectedSeats;

    public MovieBookingGUI() {
        setTitle("Movie Ticket Booking");
        setSize(500, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // Movie selection
        JPanel topPanel = new JPanel();
        topPanel.add(new JLabel("Select Movie:"));
        String[] movies = {"Avengers: Endgame", "Inception", "The Dark Knight", "Titanic"};
        movieComboBox = new JComboBox<>(movies);
        topPanel.add(movieComboBox);

        // User details
        JPanel middlePanel = new JPanel(new GridLayout(2, 2));
        middlePanel.add(new JLabel("Name:"));
        nameField = new JTextField(20);
        middlePanel.add(nameField);
        middlePanel.add(new JLabel("Number of Tickets:"));
        ticketCountField = new JTextField(5);
        middlePanel.add(ticketCountField);

        // Seat selection (simple grid of 10 seats)
        seatPanel = new JPanel(new GridLayout(2, 5));
        seatButtons = new ArrayList<>();
        selectedSeats = new ArrayList<>();
        for (int i = 1; i <= 10; i++) {
            JButton seatButton = new JButton("Seat " + i);
            seatButton.addActionListener(new SeatActionListener());
            seatButtons.add(seatButton);
            seatPanel.add(seatButton);
        }

        // Book button
        bookButton = new JButton("Book Tickets");
        bookButton.addActionListener(new BookActionListener());

        add(topPanel, BorderLayout.NORTH);
        add(middlePanel, BorderLayout.CENTER);
        add(seatPanel, BorderLayout.SOUTH);
        add(bookButton, BorderLayout.EAST);

        setVisible(true);
    }

    private class SeatActionListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            JButton button = (JButton) e.getSource();
            if (selectedSeats.contains(button.getText())) {
                selectedSeats.remove(button.getText());
                button.setBackground(null);
            } else {
                selectedSeats.add(button.getText());
                button.setBackground(Color.GREEN);
            }
        }
    }

    private class BookActionListener implements ActionListener {
        @Override
        public void actionPerformed(ActionEvent e) {
            String name = nameField.getText();
            String movie = (String) movieComboBox.getSelectedItem();
            int ticketCount;
            try {
                ticketCount = Integer.parseInt(ticketCountField.getText());
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(null, "Invalid ticket count!");
                return;
            }

            if (selectedSeats.size() != ticketCount) {
                JOptionPane.showMessageDialog(null, "Selected seats must match ticket count!");
                return;
            }

            String summary = "Booking Confirmed!\nName: " + name + "\nMovie: " + movie + "\nSeats: " + String.join(", ", selectedSeats);
            JOptionPane.showMessageDialog(null, summary);
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(MovieBookingGUI::new);
    }
}
