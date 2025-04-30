# Database Connectivity

The application connects to an Oracle database to store and retrieve student and marks data. It uses JDBC for database operations with the following configuration:
- **URL**: `jdbc:oracle:thin:@localhost:1521:XE`
- **User**: `student_user`
- **Password**: Configurable

Key database operations include:
- Querying student records with dynamic SQL for search functionality.
- Inserting, updating, and deleting student and marks records.
- Using prepared statements to prevent SQL injection.
- Transaction management with `commit` and `rollback` for data consistency.

The database schema includes:
- `students` table: Stores student details (student_id, roll_number, name, class_name).
- `marks` table: Stores marks for each subject per student (mark_id, student_id, subject, marks_obtained).

## Example Code Snippet
```java
private void saveStudent() {
    try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD)) {
        conn.setAutoCommit(false);
        int studentId = getStudentId(conn, rollNo);
        if (studentId == -1) {
            String insertStudentSql = "INSERT INTO students (student_id, roll_number, name, class_name) " +
                                     "VALUES (student_id_seq.NEXTVAL, ?, ?, ?)";
            try (PreparedStatement pstmt = conn.prepareStatement(insertStudentSql, new String[] {"STUDENT_ID"})) {
                pstmt.setString(1, rollNo);
                pstmt.setString(2, name);
                pstmt.setString(3, className);
                pstmt.executeUpdate();
                try (ResultSet rs = pstmt.getGeneratedKeys()) {
                    if (rs.next()) studentId = rs.getInt(1);
                }
            }
        }
        conn.commit();
    } catch (SQLException e) {
        showErrorMessage("Database error: " + e.getMessage());
    }
}
```