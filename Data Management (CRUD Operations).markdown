# Data Management (CRUD Operations)

The application supports Create, Read, Update, and Delete (CRUD) operations for managing student records and marks:

- **Create**: Add new students and their marks via the "Add/Edit Student" tab.
- **Read**: Search and display student records in the table.
- **Update**: Modify existing student details and marks by selecting a record and saving changes.
- **Delete**: Remove a student and their associated marks with confirmation.

The `saveStudent` method handles both insert and update operations, validating input and managing database transactions. The `deleteSelectedStudent` method ensures associated marks are deleted before removing the student record.

## Example Code Snippet
```java
private void deleteSelectedStudent() {
    int selectedRow = resultTable.getSelectedRow();
    if (selectedRow == -1) {
        showErrorMessage("Please select a student to delete");
        return;
    }
    String rollNo = tableModel.getValueAt(selectedRow, 0).toString();
    try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD)) {
        conn.setAutoCommit(false);
        int studentId = getStudentId(conn, rollNo);
        try (PreparedStatement pstmt = conn.prepareStatement("DELETE FROM marks WHERE student_id = ?")) {
            pstmt.setInt(1, studentId);
            pstmt.executeUpdate();
        }
        try (PreparedStatement pstmt = conn.prepareStatement("DELETE FROM students WHERE student_id = ?")) {
            pstmt.setInt(1, studentId);
            pstmt.executeUpdate();
        }
        conn.commit();
        tableModel.removeRow(selectedRow);
    } catch (SQLException e) {
        showErrorMessage("Database error: " + e.getMessage());
    }
}
```