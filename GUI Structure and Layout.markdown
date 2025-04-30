# GUI Structure and Layout

The `EnhancedMarksReport` class creates a graphical user interface (GUI) using Java Swing for managing student marks. The GUI is organized into a tabbed interface with two main panels:

- **Search & View Tab**: Allows users to search for student records, view results in a table, and perform actions like sorting, updating, deleting, or exporting data.
- **Add/Edit Student Tab**: Provides a form for entering or updating student details and marks for six subjects.

Key components include:
- `JTabbedPane` for tab navigation.
- `JTable` for displaying search results with columns for roll number, name, class, subject marks, total, percentage, and rank.
- Input fields (`JTextField`, `JComboBox`) for search criteria and data entry.
- Buttons for actions like Search, Export, Clear, Save, etc.
- A status label for user feedback.

The layout is managed using `BorderLayout`, `GridBagLayout`, and `GridLayout` to ensure a clean and organized presentation.

## Example Code Snippet
```java
public EnhancedMarksReport() {
    setTitle("Student Marks Management System");
    setSize(900, 650);
    setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    setLocationRelativeTo(null);

    initComponents();
    setupLayout();
    setVisible(true);
}

private void setupLayout() {
    JPanel searchPanel = createSearchPanel();
    JPanel dataEntryPanel = createDataEntryPanel();
    tabbedPane.addTab("Search & View", createViewPanel(searchPanel));
    tabbedPane.addTab("Add/Edit Student", dataEntryPanel);
    setLayout(new BorderLayout());
    add(tabbedPane, BorderLayout.CENTER);
    JPanel statusPanel = new JPanel(new BorderLayout());
    statusPanel.add(statusLabel, BorderLayout.WEST);
    add(statusPanel, BorderLayout.SOUTH);
}
```