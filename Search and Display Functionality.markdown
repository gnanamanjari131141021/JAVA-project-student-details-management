# Search and Display Functionality

The search functionality allows users to filter student records based on roll number, name, class, subject, and marks. The results are displayed in a `JTable` with calculated fields for total marks, percentage, and rank.

Key features:
- Dynamic SQL query construction using a pivot-like approach to aggregate marks for each subject.
- Support for partial matches using `LIKE` and case-insensitive searches.
- Sorting by total marks and ranking students based on their totals.
- Selection of a table row populates the edit form for updates.

The `processResults` method handles result set processing, calculates totals and percentages, and assigns ranks based on total marks.

## Example Code Snippet
```java
private void searchRecords(String subject) {
    StringBuilder queryBuilder = new StringBuilder(
        "SELECT s.student_id, s.roll_number, s.name, s.class_name, " +
        "MAX(CASE WHEN m.subject = 'Mathematics' THEN NVL(TO_NUMBER(m.marks_obtained), 0) ELSE 0 END) AS Mathematics, " +
        // ... other subjects
        "FROM students s LEFT JOIN marks m ON s.student_id = m.student_id WHERE 1=1 "
    );
    List<String> conditions = new ArrayList<>();
    List<Object> parameters = new ArrayList<>();
    if (!roll.isEmpty()) {
        conditions.add("AND LOWER(s.roll_number) LIKE LOWER(?)");
        parameters.add("%" + roll + "%");
    }
    // ... other conditions
    try (Connection conn = DriverManager.getConnection(URL, USER, PASSWORD);
         PreparedStatement pstmt = conn.prepareStatement(queryBuilder.toString())) {
        for (int i = 0; i < parameters.size(); i++) {
            pstmt.setObject(i + 1, parameters.get(i));
        }
        ResultSet rs = pstmt.executeQuery();
        processResults(rs);
    } catch (SQLException e) {
        showErrorMessage("⚠ Database error: " + e.getMessage());
    }
}
```