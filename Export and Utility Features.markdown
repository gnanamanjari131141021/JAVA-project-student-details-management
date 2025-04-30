# Export and Utility Features

The application includes utility features to enhance usability:

- **Export to CSV**: Users can export selected or all table records to a CSV file, filtered by roll number and name.
- **Clear Form**: Resets input fields and the table for a fresh start.
- **Sort by Total**: Reorders the table by total marks, updating ranks accordingly.
- **Error Handling**: Displays user-friendly error messages and status updates via a status label and dialog boxes.
- **Look and Feel**: Uses the Nimbus look and feel for a modern appearance, with fallback to the cross-platform default.

The `exportResults` method handles CSV export, prompting for filters and saving the file using `JFileChooser`.

## Example Code Snippet
```java
private void exportResults() {
    String roll = JOptionPane.showInputDialog(this, "Enter Roll Number (leave blank for all):");
    String name = JOptionPane.showInputDialog(this, "Enter Student Name (leave blank for all):");
    List<Integer> rowsToExport = new ArrayList<>();
    for (int i = 0; i < tableModel.getRowCount(); i++) {
        String rowRoll = (String) tableModel.getValueAt(i, 0);
        String rowName = (String) tableModel.getValueAt(i, 1);
        if ((roll.isEmpty() || rowRoll.toLowerCase().contains(roll.toLowerCase())) &&
            (name.isEmpty() || rowName.toLowerCase().contains(name.toLowerCase()))) {
            rowsToExport.add(i);
        }
    }
    JFileChooser fileChooser = new JFileChooser();
    if (fileChooser.showSaveDialog(this) == JFileChooser.APPROVE_OPTION) {
        String filePath = fileChooser.getSelectedFile().getAbsolutePath();
        if (!filePath.toLowerCase().endsWith(".csv")) filePath += ".csv";
        try (PrintWriter writer = new PrintWriter(new FileWriter(filePath))) {
            writer.println("Roll Number,Name,Class,Mathematics,Science,English,History,Geography,Computer Science,Total,Percentage,Rank");
            for (int rowIndex : rowsToExport) {
                StringBuilder line = new StringBuilder();
                for (int j = 0; j < tableModel.getColumnCount(); j++) {
                    line.append(tableModel.getValueAt(rowIndex, j));
                    if (j < tableModel.getColumnCount() - 1) line.append(",");
                }
                writer.println(line);
            }
            setStatus("✅ Successfully exported " + rowsToExport.size() + " records to " + filePath, false);
        } catch (IOException e) {
            showErrorMessage("Export failed: " + e.getMessage());
        }
    }
}
```