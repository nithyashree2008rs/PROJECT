package daoimpl;

import dao.*;
import database.DBConnection;
import model.*;

import java.sql.*;
import java.time.LocalDate;
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;

public class LibraryDAOImpl implements
        StudentDAO, BookDAO, IssueDAO, ReturnDAO, FineDAO {

    // ============== STUDENT MANAGEMENT ==============

    @Override
    public void addStudent(Student s) {
        String sql = "INSERT INTO students VALUES (?, ?, ?, ?)";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql)) {

            ps.setString(1, s.getStudentId());
            ps.setString(2, s.getName());
            ps.setString(3, s.getDepartment());
            ps.setString(4, s.getPhone());

            ps.executeUpdate();

        } catch (SQLException e) {
            throw new RuntimeException("Unable to add student", e);
        }
    }

    @Override
    public void updateStudent(Student s) {
        String sql = "UPDATE students SET student_name=?, " +
                     "department=?, phone=? WHERE student_id=?";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql)) {

            ps.setString(1, s.getName());
            ps.setString(2, s.getDepartment());
            ps.setString(3, s.getPhone());
            ps.setString(4, s.getStudentId());

            if (ps.executeUpdate() == 0) {
                throw new IllegalArgumentException("Student not found");
            }

        } catch (SQLException e) {
            throw new RuntimeException("Unable to update student", e);
        }
    }

    @Override
    public void deleteStudent(String id) {
        String sql = "DELETE FROM students WHERE student_id=?";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql)) {

            ps.setString(1, id);

            if (ps.executeUpdate() == 0) {
                throw new IllegalArgumentException("Student not found");
            }

        } catch (SQLException e) {
            throw new RuntimeException(
                "Unable to delete student. Check borrowing records.", e);
        }
    }

    @Override
    public Student getStudent(String id) {
        String sql = "SELECT * FROM students WHERE student_id=?";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql)) {

            ps.setString(1, id);

            try (ResultSet rs = ps.executeQuery()) {
                if (rs.next()) {
                    return mapStudent(rs);
                }
            }

        } catch (SQLException e) {
            throw new RuntimeException("Unable to find student", e);
        }

        return null;
    }

    @Override
    public List<Student> getAllStudents() {
        List<Student> list = new ArrayList<>();
        String sql = "SELECT * FROM students ORDER BY student_name";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql);
             ResultSet rs = ps.executeQuery()) {

            while (rs.next()) {
                list.add(mapStudent(rs));
            }

        } catch (SQLException e) {
            throw new RuntimeException("Unable to load students", e);
        }

        return list;
    }

    private Student mapStudent(ResultSet rs) throws SQLException {
        return new Student(
            rs.getString("student_id"),
            rs.getString("student_name"),
            rs.getString("department"),
            rs.getString("phone")
        );
    }

    // ============== BOOK MANAGEMENT ==============

    @Override
    public void addBook(Book b) {
        String sql = "INSERT INTO books " +
                     "(book_id, book_name, author, category, status) " +
                     "VALUES (?, ?, ?, ?, 'Available')";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql)) {

            ps.setString(1, b.getBookId());
            ps.setString(2, b.getBookName());
            ps.setString(3, b.getAuthor());
            ps.setString(4, b.getCategory());

            ps.executeUpdate();

        } catch (SQLException e) {
            throw new RuntimeException("Unable to add book", e);
        }
    }

    @Override
    public void updateBook(Book b) {
        String sql = "UPDATE books SET book_name=?, author=?, " +
                     "category=? WHERE book_id=?";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql)) {

            ps.setString(1, b.getBookName());
            ps.setString(2, b.getAuthor());
            ps.setString(3, b.getCategory());
            ps.setString(4, b.getBookId());

            if (ps.executeUpdate() == 0) {
                throw new IllegalArgumentException("Book not found");
            }

        } catch (SQLException e) {
            throw new RuntimeException("Unable to update book", e);
        }
    }

    @Override
    public void deleteBook(String id) {
        String sql = "DELETE FROM books WHERE book_id=?";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql)) {

            ps.setString(1, id);

            if (ps.executeUpdate() == 0) {
                throw new IllegalArgumentException("Book not found");
            }

        } catch (SQLException e) {
            throw new RuntimeException(
                "Unable to delete book. Check its issue history.", e);
        }
    }

    @Override
    public Book getBook(String id) {
        String sql = "SELECT * FROM books WHERE book_id=?";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql)) {

            ps.setString(1, id);

            try (ResultSet rs = ps.executeQuery()) {
                if (rs.next()) {
                    return mapBook(rs);
                }
            }

        } catch (SQLException e) {
            throw new RuntimeException("Unable to find book", e);
        }

        return null;
    }

    @Override
    public List<Book> getAllBooks() {
        List<Book> list = new ArrayList<>();
        String sql = "SELECT * FROM books ORDER BY book_name";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql);
             ResultSet rs = ps.executeQuery()) {

            while (rs.next()) {
                list.add(mapBook(rs));
            }

        } catch (SQLException e) {
            throw new RuntimeException("Unable to load books", e);
        }

        return list;
    }

    @Override
    public List<Book> searchBooks(String keyword) {
        List<Book> list = new ArrayList<>();

        String sql = "SELECT * FROM books WHERE " +
                     "book_name LIKE ? OR author LIKE ? " +
                     "OR category LIKE ? ORDER BY book_name";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql)) {

            String search = "%" + keyword + "%";
            ps.setString(1, search);
            ps.setString(2, search);
            ps.setString(3, search);

            try (ResultSet rs = ps.executeQuery()) {
                while (rs.next()) {
                    list.add(mapBook(rs));
                }
            }

        } catch (SQLException e) {
            throw new RuntimeException("Unable to search books", e);
        }

        return list;
    }

    private Book mapBook(ResultSet rs) throws SQLException {
        return new Book(
            rs.getString("book_id"),
            rs.getString("book_name"),
            rs.getString("author"),
            rs.getString("category"),
            rs.getString("status")
        );
    }

    // ============== ISSUE BOOK ==============

    @Override
    public int issueBook(String studentId, String bookId,
                         int loanDays) {

        if (loanDays <= 0) {
            throw new IllegalArgumentException(
                "Loan days must be greater than zero");
        }

        String studentSql =
            "SELECT student_id FROM students WHERE student_id=?";

        String bookSql =
            "SELECT status FROM books WHERE book_id=? FOR UPDATE";

        String issueSql =
            "INSERT INTO issue_books " +
            "(student_id, book_id, issue_date, due_date, status) " +
            "VALUES (?, ?, ?, ?, 'Issued')";

        String updateBookSql =
            "UPDATE books SET status='Issued' " +
            "WHERE book_id=? AND status='Available'";

        try (Connection con = DBConnection.getConnection()) {
            con.setAutoCommit(false);

            try {
                // Verify student
                try (PreparedStatement ps =
                         con.prepareStatement(studentSql)) {
                    ps.setString(1, studentId);

                    try (ResultSet rs = ps.executeQuery()) {
                        if (!rs.next()) {
                            throw new IllegalArgumentException(
                                "Student does not exist");
                        }
                    }
                }

                // Lock and verify book
                try (PreparedStatement ps =
                         con.prepareStatement(bookSql)) {
                    ps.setString(1, bookId);

                    try (ResultSet rs = ps.executeQuery()) {
                        if (!rs.next()) {
                            throw new IllegalArgumentException(
                                "Book does not exist");
                        }

                        if (!"Available".equals(rs.getString("status"))) {
                            throw new IllegalStateException(
                                "Book is not available");
                        }
                    }
                }

                LocalDate issueDate = LocalDate.now();
                LocalDate dueDate = issueDate.plusDays(loanDays);

                // Save issue record
                int issueId;

                try (PreparedStatement ps = con.prepareStatement(
                        issueSql, Statement.RETURN_GENERATED_KEYS)) {

                    ps.setString(1, studentId);
                    ps.setString(2, bookId);
                    ps.setDate(3, Date.valueOf(issueDate));
                    ps.setDate(4, Date.valueOf(dueDate));

                    ps.executeUpdate();

                    try (ResultSet keys = ps.getGeneratedKeys()) {
                        if (!keys.next()) {
                            throw new SQLException(
                                "Issue ID was not generated");
                        }
                        issueId = keys.getInt(1);
                    }
                }

                // Update book availability
                try (PreparedStatement ps =
                         con.prepareStatement(updateBookSql)) {
                    ps.setString(1, bookId);

                    if (ps.executeUpdate() != 1) {
                        throw new IllegalStateException(
                            "Unable to update book status");
                    }
                }

                con.commit();
                return issueId;

            } catch (Exception e) {
                con.rollback();
                throw e;
            } finally {
                con.setAutoCommit(true);
            }

        } catch (Exception e) {
            throw new RuntimeException("Book issue failed", e);
        }
    }

    @Override
    public List<IssueBook> getAllIssues() {
        List<IssueBook> list = new ArrayList<>();

        String sql = "SELECT * FROM issue_books ORDER BY issue_id DESC";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql);
             ResultSet rs = ps.executeQuery()) {

            while (rs.next()) {
                list.add(mapIssue(rs));
            }

        } catch (SQLException e) {
            throw new RuntimeException("Unable to load issues", e);
        }

        return list;
    }

    @Override
    public List<IssueBook> getStudentHistory(String studentId) {
        List<IssueBook> list = new ArrayList<>();

        String sql = "SELECT * FROM issue_books " +
                     "WHERE student_id=? ORDER BY issue_date DESC";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql)) {

            ps.setString(1, studentId);

            try (ResultSet rs = ps.executeQuery()) {
                while (rs.next()) {
                    list.add(mapIssue(rs));
                }
            }

        } catch (SQLException e) {
            throw new RuntimeException(
                "Unable to load borrowing history", e);
        }

        return list;
    }

    private IssueBook mapIssue(ResultSet rs) throws SQLException {
        return new IssueBook(
            rs.getInt("issue_id"),
            rs.getString("student_id"),
            rs.getString("book_id"),
            rs.getDate("issue_date").toLocalDate(),
            rs.getDate("due_date").toLocalDate(),
            rs.getString("status")
        );
    }

    // ============== RETURN BOOK & FINE ==============

    @Override
    public BigDecimal returnBook(int issueId,
                                 LocalDate returnDate) {

        final BigDecimal dailyFine = new BigDecimal("10.00");

        String issueSql =
            "SELECT book_id, due_date, status FROM issue_books " +
            "WHERE issue_id=? FOR UPDATE";

        String returnSql =
            "INSERT INTO return_books (issue_id, return_date) " +
            "VALUES (?, ?)";

        String updateIssueSql =
            "UPDATE issue_books SET status='Returned' " +
            "WHERE issue_id=? AND status='Issued'";

        String updateBookSql =
            "UPDATE books SET status='Available' WHERE book_id=?";

        String fineSql =
            "INSERT INTO fines (issue_id, fine_amount, payment_status) " +
            "VALUES (?, ?, ?)";

        try (Connection con = DBConnection.getConnection()) {
            con.setAutoCommit(false);

            try {
                String bookId;
                LocalDate dueDate;

                try (PreparedStatement ps =
                         con.prepareStatement(issueSql)) {
                    ps.setInt(1, issueId);

                    try (ResultSet rs = ps.executeQuery()) {
                        if (!rs.next()) {
                            throw new IllegalArgumentException(
                                "Issue record not found");
                        }

                        if (!"Issued".equals(rs.getString("status"))) {
                            throw new IllegalStateException(
                                "Book has already been returned");
                        }

                        bookId = rs.getString("book_id");
                        dueDate = rs.getDate("due_date").toLocalDate();
                    }
                }

                long overdueDays = Math.max(
                    0, java.time.temporal.ChronoUnit.DAYS.between(
                        dueDate, returnDate)
                );

                BigDecimal amount = dailyFine.multiply(
                    BigDecimal.valueOf(overdueDays)
                );

                try (PreparedStatement ps =
                         con.prepareStatement(returnSql)) {
                    ps.setInt(1, issueId);
                    ps.setDate(2, Date.valueOf(returnDate));
                    ps.executeUpdate();
                }

                try (PreparedStatement ps =
                         con.prepareStatement(updateIssueSql)) {
                    ps.setInt(1, issueId);

                    if (ps.executeUpdate() != 1) {
                        throw new IllegalStateException(
                            "Issue status could not be updated");
                    }
                }

                try (PreparedStatement ps =
                         con.prepareStatement(updateBookSql)) {
                    ps.setString(1, bookId);
                    ps.executeUpdate();
                }

                String paymentStatus =
                    amount.compareTo(BigDecimal.ZERO) == 0
                        ? "Paid" : "Pending";

                try (PreparedStatement ps =
                         con.prepareStatement(fineSql)) {
                    ps.setInt(1, issueId);
                    ps.setBigDecimal(2, amount);
                    ps.setString(3, paymentStatus);
                    ps.executeUpdate();
                }

                con.commit();
                return amount;

            } catch (Exception e) {
                con.rollback();
                throw e;
            } finally {
                con.setAutoCommit(true);
            }

        } catch (Exception e) {
            throw new RuntimeException("Book return failed", e);
        }
    }

    // ============== FINE MANAGEMENT ==============

    @Override
    public List<Fine> getAllFines() {
        List<Fine> list = new ArrayList<>();

        String sql = "SELECT * FROM fines ORDER BY fine_id DESC";

        try (Connection con = DBConnection.getConnection();
             PreparedStatement ps = con.prepareStatement(sql);
             ResultSet rs = ps.executeQuery()) {

            while (rs.next()) {
                list.add(new Fine(
                    rs.getInt("fine_id"),
                    rs.getInt("issue_id"),
                    rs.getBigDecimal("fine_amount"),
                    rs.getString("payment_status")
                ));
            }

        } catch (SQLException e) {
            throw new RuntimeException("Unable to load fines", e);
        }

        return list;
    }

    @Override
    public BigDecimal payFine(int fineId, BigDecimal amount) {
        if (amount == null || amount.compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException(
                "Payment must be greater than zero");
        }

        try (Connection con = DBConnection.getConnection()) {
            con.setAutoCommit(false);

            try {
                BigDecimal total;
                BigDecimal alreadyPaid;

                String sql = "SELECT fine_amount, payment_status " +
                    "FROM fines WHERE fine_id=? FOR UPDATE";

                try (PreparedStatement ps = con.prepareStatement(sql)) {
                    ps.setInt(1, fineId);

                    try (ResultSet rs = ps.executeQuery()) {
                        if (!rs.next()) {
                            throw new IllegalArgumentException(
                                "Fine record not found");
                        }

                        total = rs.getBigDecimal("fine_amount");

                        String status = rs.getString("payment_status");
                        if ("Paid".equals(status)) {
                            throw new IllegalStateException(
                                "Fine is already paid");
                        }
                    }
                }

                String paidSql =
                    "SELECT COALESCE(SUM(amount), 0) AS paid " +
                    "FROM fine_payments WHERE fine_id=?";

                try (PreparedStatement ps =
                         con.prepareStatement(paidSql)) {
                    ps.setInt(1, fineId);

                    try (ResultSet rs = ps.executeQuery()) {
                        rs.next();
                        alreadyPaid = rs.getBigDecimal("paid");
                    }
                }
