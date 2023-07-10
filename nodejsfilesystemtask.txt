const express = require('express');
const fs = require('fs');
const path = require('path');

const app = express();
const folderPath = '/path/to/folder'; // Replace with the desired folder path

// API endpoint to create a text file with the current timestamp
app.post('/create_file', (req, res) => {
  const currentDateTime = new Date().toISOString().replace(/:/g, '-');
  const filename = `${currentDateTime}.txt`;
  const filePath = path.join(folderPath, filename);
  const fileContent = new Date().toString();

  fs.writeFile(filePath, fileContent, (err) => {
    if (err) {
      console.error(err);
      return res.status(500).json({ error: 'Failed to create file.' });
    }
    res.status(201).json({ message: `Text file created: ${filename}` });
  });
});

// API endpoint to retrieve all text files in the folder
app.get('/get_files', (req, res) => {
  fs.readdir(folderPath, (err, files) => {
    if (err) {
      console.error(err);
      return res.status(500).json({ error: 'Failed to retrieve files.' });
    }
    const textFiles = files.filter((file) => path.extname(file) === '.txt');
    res.status(200).json({ files: textFiles });
  });
});

app.listen(3000, () => {
  console.log('Server started on port 3000');
});
