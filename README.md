import javax.swing.*;
import javax.swing.border.EmptyBorder;
import javax.swing.table.DefaultTableModel;
import java.awt.*;

public class LibraryMonitoringSystem extends JFrame {

    // Main colors
    Color darkBlue = new Color(30, 58, 95);
    Color blue = new Color(41, 128, 185);
    Color lightBlue = new Color(236, 244, 252);
    Color white = Color.WHITE;
    Color darkText = new Color(40, 40, 40);

    JPanel contentPanel;
    CardLayout cardLayout;

    public LibraryMonitoringSystem() {

        setTitle("Library Monitoring System");
        setSize(1200, 700);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        // Main layout
        setLayout(new BorderLayout());

        // Sidebar
        JPanel sidebar = createSidebar();
        add(sidebar, BorderLayout.WEST);

        // Main content
        cardLayout = new CardLayout();
        contentPanel = new JPanel(cardLayout);

        contentPanel.add(createDashboard(), "Dashboard");
        contentPanel.add(createStudentsPanel(), "Students");
        contentPanel.add(createBooksPanel(), "Books");
        contentPanel.add(createIssuePanel(), "Issue");
        contentPanel.add(createReturnPanel(), "Return");
        contentPanel.add(createFinePanel(), "Fine");
        contentPanel.add(createReportsPanel(), "Reports");

        add(contentPanel, BorderLayout.CENTER);

        // Show dashboard first
        cardLayout.show(contentPanel, "Dashboard");
    }

    // ================= SIDEBAR =================

    private JPanel createSidebar() {

        JPanel sidebar = new JPanel();
        sidebar.setPreferredSize(new Dimension(230, 700));
        sidebar.setBackground(darkBlue);
        sidebar.setLayout(new BorderLayout());

        // Logo / Title
        JPanel titlePanel = new JPanel();
        titlePanel.setBackground(darkBlue);
        titlePanel.setLayout(new BoxLayout(titlePanel, BoxLayout.Y_AXIS));
        titlePanel.setBorder(new EmptyBorder(25, 15, 25, 15));

        JLabel libraryIcon = new JLabel("📚");
        libraryIcon.setFont(new Font("Arial", Font.PLAIN, 35));
        libraryIcon.setAlignmentX(Component.CENTER_ALIGNMENT);

        JLabel title = new JLabel("LIBRARY");
        title.setForeground(Color.WHITE);
        title.setFont(new Font("Arial", Font.BOLD, 22));
        title.setAlignmentX(Component.CENTER_ALIGNMENT);

        JLabel subtitle = new JLabel("MONITORING SYSTEM");
        subtitle.setForeground(new Color(190, 210, 230));
        subtitle.setFont(new Font("Arial", Font.BOLD, 11));
        subtitle.setAlignmentX(Component.CENTER_ALIGNMENT);

        titlePanel.add(libraryIcon);
        titlePanel.add(title);
        titlePanel.add(subtitle);

        sidebar.add(titlePanel, BorderLayout.NORTH);

        // Menu
        JPanel menuPanel = new JPanel();
        menuPanel.setBackground(darkBlue);
        menuPanel.setLayout(new GridLayout(8, 1, 0, 8));
        menuPanel.setBorder(new EmptyBorder(10, 12, 10, 12));

        JButton dashboardBtn = createMenuButton("🏠  Dashboard");
        JButton studentBtn = createMenuButton("👨‍🎓  Students");
        JButton bookBtn = createMenuButton("📚  Books");
        JButton issueBtn = createMenuButton("📖  Issue Book");
        JButton returnBtn = createMenuButton("↩  Return Book");
        JButton fineBtn = createMenuButton("💰  Fine Management");
        JButton reportBtn = createMenuButton("📊  Reports");
        JButton logoutBtn = createMenuButton("🚪  Logout");

        dashboardBtn.addActionListener(e ->
                cardLayout.show(contentPanel, "Dashboard"));

        studentBtn.addActionListener(e ->
                cardLayout.show(contentPanel, "Students"));

        bookBtn.addActionListener(e ->
                cardLayout.show(contentPanel, "Books"));

        issueBtn.addActionListener(e ->
                cardLayout.show(contentPanel, "Issue"));

        returnBtn.addActionListener(e ->
                cardLayout.show(contentPanel, "Return"));

        fineBtn.addActionListener(e ->
                cardLayout.show(contentPanel, "Fine"));

        reportBtn.addActionListener(e ->
                cardLayout.show(contentPanel, "Reports"));

        logoutBtn.addActionListener(e -> {

            int choice = JOptionPane.showConfirmDialog(
                    this,
                    "Are you sure you want to logout?",
                    "Logout",
                    JOptionPane.YES_NO_OPTION
            );

            if (choice == JOptionPane.YES_OPTION) {
                System.exit(0);
            }
        });

        menuPanel.add(dashboardBtn);
        menuPanel.add(studentBtn);
        menuPanel.add(bookBtn);
        menuPanel.add(issueBtn);
        menuPanel.add(returnBtn);
        menuPanel.add(fineBtn);
        menuPanel.add(reportBtn);
        menuPanel.add(logoutBtn);

        sidebar.add(menuPanel, BorderLayout.CENTER);

        return sidebar;
    }

    private JButton createMenuButton(String text) {

        JButton button = new JButton(text);

        button.setFont(new Font("Arial", Font.BOLD, 14));
        button.setForeground(Color.WHITE);
        button.setBackground(darkBlue);
        button.setFocusPainted(false);
        button.setBorderPainted(false);
        button.setHorizontalAlignment(SwingConstants.LEFT);
        button.setCursor(new Cursor(Cursor.HAND_CURSOR));

        return button;
    }

    // ================= DASHBOARD =================

    private JPanel createDashboard() {

        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(lightBlue);
        panel.setBorder(new EmptyBorder(25, 30, 25, 30));

        JLabel heading = new JLabel("Library Dashboard");
        heading.setFont(new Font("Arial", Font.BOLD, 28));
        heading.setForeground(darkText);

        JLabel welcome = new JLabel(
                "Welcome to Library Monitoring System"
        );
        welcome.setFont(new Font("Arial", Font.PLAIN, 15));

        JPanel header = new JPanel(new BorderLayout());
        header.setOpaque(false);

        header.add(heading, BorderLayout.NORTH);
        header.add(welcome, BorderLayout.SOUTH);

        panel.add(header, BorderLayout.NORTH);

        // Cards
        JPanel cards = new JPanel(new GridLayout(2, 2, 20, 20));
        cards.setOpaque(false);
        cards.setBorder(new EmptyBorder(30, 0, 30, 0));

        cards.add(createDashboardCard(
                "Total Books",
                "1250",
                blue
        ));

        cards.add(createDashboardCard(
                "Registered Students",
                "450",
                new Color(39, 174, 96)
        ));

        cards.add(createDashboardCard(
                "Books Issued",
                "125",
                new Color(243, 156, 18)
        ));

        cards.add(createDashboardCard(
                "Overdue Books",
                "18",
                new Color(192, 57, 43)
        ));

        panel.add(cards, BorderLayout.CENTER);

        return panel;
    }

    private JPanel createDashboardCard(
            String title,
            String value,
            Color color) {

        JPanel card = new JPanel();
        card.setBackground(Color.WHITE);
        card.setLayout(new BorderLayout());
        card.setBorder(BorderFactory.createLineBorder(
                new Color(220, 220, 220)
        ));

        JLabel titleLabel = new JLabel(title);
        titleLabel.setFont(new Font("Arial", Font.BOLD, 16));
        titleLabel.setBorder(new EmptyBorder(20, 20, 5, 20));

        JLabel valueLabel = new JLabel(value);
        valueLabel.setFont(new Font("Arial", Font.BOLD, 35));
        valueLabel.setForeground(color);
        valueLabel.setBorder(new EmptyBorder(5, 20, 20, 20));

        card.add(titleLabel, BorderLayout.NORTH);
        card.add(valueLabel, BorderLayout.CENTER);

        return card;
    }

    // ================= STUDENTS =================

    private JPanel createStudentsPanel() {

        JPanel panel = createMainPanel("Student Management");

        JPanel form = new JPanel(new GridLayout(2, 4, 10, 10));
        form.setBackground(Color.WHITE);
        form.setBorder(new EmptyBorder(15, 15, 15, 15));

        JTextField idField = new JTextField();
        JTextField nameField = new JTextField();
        JTextField departmentField = new JTextField();
        JTextField phoneField = new JTextField();

        form.add(new JLabel("Student ID"));
        form.add(new JLabel("Student Name"));
        form.add(new JLabel("Department"));
        form.add(new JLabel("Phone Number"));

        form.add(idField);
        form.add(nameField);
        form.add(departmentField);
        form.add(phoneField);

        JButton addButton = new JButton("Add Student");
        addButton.setBackground(blue);
        addButton.setForeground(Color.WHITE);

        String[] columns = {
                "Student ID",
                "Student Name",
                "Department",
                "Phone Number"
        };

        DefaultTableModel model =
                new DefaultTableModel(columns, 0);

        JTable table = new JTable(model);

        addButton.addActionListener(e -> {

            if (idField.getText().isEmpty()
                    || nameField.getText().isEmpty()) {

                JOptionPane.showMessageDialog(
                        this,
                        "Please enter Student ID and Name."
                );

            } else {

                model.addRow(new Object[]{
                        idField.getText(),
                        nameField.getText(),
                        departmentField.getText(),
                        phoneField.getText()
                });

                idField.setText("");
                nameField.setText("");
                departmentField.setText("");
                phoneField.setText("");
            }
        });

        JPanel top = new JPanel(new BorderLayout());
        top.setBackground(Color.WHITE);
        top.add(form, BorderLayout.CENTER);
        top.add(addButton, BorderLayout.SOUTH);

        panel.add(top, BorderLayout.NORTH);
        panel.add(new JScrollPane(table), BorderLayout.CENTER);

        return panel;
    }

    // ================= BOOKS =================

    private JPanel createBooksPanel() {

        JPanel panel = createMainPanel("Book Management");

        JPanel form = new JPanel(new GridLayout(2, 4, 10, 10));
        form.setBackground(Color.WHITE);
        form.setBorder(new EmptyBorder(15, 15, 15, 15));

        JTextField bookId = new JTextField();
        JTextField bookName = new JTextField();
        JTextField author = new JTextField();
        JTextField category = new JTextField();

        form.add(new JLabel("Book ID"));
        form.add(new JLabel("Book Name"));
        form.add(new JLabel("Author"));
        form.add(new JLabel("Category"));

        form.add(bookId);
        form.add(bookName);
        form.add(author);
        form.add(category);

        JButton addButton = new JButton("Add Book");
        addButton.setBackground(blue);
        addButton.setForeground(Color.WHITE);

        String[] columns = {
                "Book ID",
                "Book Name",
                "Author",
                "Category",
                "Status"
        };

        DefaultTableModel model =
                new DefaultTableModel(columns, 0);

        JTable table = new JTable(model);

        addButton.addActionListener(e -> {

            if (bookId.getText().isEmpty()
                    || bookName.getText().isEmpty()) {

                JOptionPane.showMessageDialog(
                        this,
                        "Please enter Book ID and Book Name."
                );

            } else {

                model.addRow(new Object[]{
                        bookId.getText(),
                        bookName.getText(),
                        author.getText(),
                        category.getText(),
                        "Available"
                });

                bookId.setText("");
                bookName.setText("");
                author.setText("");
                category.setText("");
            }
        });

        JPanel top = new JPanel(new BorderLayout());
        top.setBackground(Color.WHITE);
        top.add(form, BorderLayout.CENTER);
        top.add(addButton, BorderLayout.SOUTH);

        panel.add(top, BorderLayout.NORTH);
        panel.add(new JScrollPane(table), BorderLayout.CENTER);

        return panel;
    }

    // ================= ISSUE BOOK =================

    private JPanel createIssuePanel() {

        JPanel panel = createMainPanel("Issue Book");

        JPanel form = new JPanel(
                new GridLayout(5, 2, 15, 15)
        );

        form.setBackground(Color.WHITE);
        form.setBorder(new EmptyBorder(40, 80, 40, 80));

        JTextField studentId = new JTextField();
        JTextField bookId = new JTextField();
        JTextField issueDate = new JTextField();
        JTextField dueDate = new JTextField();

        form.add(new JLabel("Student ID"));
        form.add(studentId);

        form.add(new JLabel("Book ID"));
        form.add(bookId);

        form.add(new JLabel("Issue Date"));
        form.add(issueDate);

        form.add(new JLabel("Due Date"));
        form.add(dueDate);

        JButton issueButton = new JButton("Issue Book");
        issueButton.setBackground(blue);
        issueButton.setForeground(Color.WHITE);

        form.add(new JLabel(""));
        form.add(issueButton);

        issueButton.addActionListener(e -> {

            JOptionPane.showMessageDialog(
                    this,
                    "Book issued successfully!"
            );
        });

        panel.add(form, BorderLayout.NORTH);

        return panel;
    }

    // ================= RETURN BOOK =================

    private JPanel createReturnPanel() {

        JPanel panel = createMainPanel("Return Book");

        JPanel form = new JPanel(
                new GridLayout(4, 2, 15, 15)
        );

        form.setBackground(Color.WHITE);
        form.setBorder(new EmptyBorder(40, 80, 40, 80));

        JTextField studentId = new JTextField();
        JTextField bookId = new JTextField();
        JTextField returnDate = new JTextField();

        form.add(new JLabel("Student ID"));
        form.add(studentId);

        form.add(new JLabel("Book ID"));
        form.add(bookId);

        form.add(new JLabel("Return Date"));
        form.add(returnDate);

        JButton returnButton = new JButton("Return Book");
        returnButton.setBackground(new Color(39, 174, 96));
        returnButton.setForeground(Color.WHITE);

        form.add(new JLabel(""));
        form.add(returnButton);

        returnButton.addActionListener(e -> {

            JOptionPane.showMessageDialog(
                    this,
                    "Book returned successfully!"
            );
        });

        panel.add(form, BorderLayout.NORTH);

        return panel;
    }

    // ================= FINE =================

    private JPanel createFinePanel() {

        JPanel panel = createMainPanel("Fine Management");

        String[] columns = {
                "Student ID",
                "Book ID",
                "Due Date",
                "Return Date",
                "Fine Amount",
                "Payment Status"
        };

        Object[][] data = {
                {"S001", "B101", "01-10-2026",
                        "05-10-2026", "₹40", "Pending"},
                {"S002", "B205", "02-10-2026",
                        "04-10-2026", "₹20", "Paid"}
        };

        JTable table = new JTable(
                new DefaultTableModel(data, columns)
        );

        panel.add(new JScrollPane(table),
                BorderLayout.CENTER);

        return panel;
    }

    // ================= REPORTS =================

    private JPanel createReportsPanel() {

        JPanel panel = createMainPanel("Reports");

        JPanel reportPanel = new JPanel(
                new GridLayout(2, 2, 20, 20)
        );

        reportPanel.setBackground(Color.WHITE);
        reportPanel.setBorder(
                new EmptyBorder(50, 80, 50, 80)
        );

        JButton issued = new JButton("Issued Books Report");
        JButton returned = new JButton("Returned Books Report");
        JButton overdue = new JButton("Overdue Books Report");
        JButton students = new JButton("Student Report");

        reportPanel.add(issued);
        reportPanel.add(returned);
        reportPanel.add(overdue);
        reportPanel.add(students);

        panel.add(reportPanel, BorderLayout.CENTER);

        return panel;
    }

    // ================= COMMON PANEL =================

    private JPanel createMainPanel(String title) {

        JPanel panel = new JPanel(new BorderLayout());
        panel.setBackground(lightBlue);
        panel.setBorder(new EmptyBorder(25, 30, 25, 30));

        JLabel heading = new JLabel(title);
        heading.setFont(new Font("Arial", Font.BOLD, 28));
        heading.setForeground(darkText);

        panel.add(heading, BorderLayout.NORTH);

        return panel;
    }

    // ================= MAIN =================

    public static void main(String[] args) {

        SwingUtilities.invokeLater(() -> {

            LibraryMonitoringSystem app =
                    new LibraryMonitoringSystem();

            app.setVisible(true);
        });
    }
}
