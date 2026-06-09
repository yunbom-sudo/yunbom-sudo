    "10001",
    "11110",
  ],
  o: [
    "01110",
    "10001",
    "10001",
    "10001",
    "10001",
    "10001",
    "01110",
  ],
  m: [
    "10001",
    "11011",
    "10101",
    "10101",
    "10001",
    "10001",
    "10001",
  ],
};

const word = "yunbom";
const letterWidth = 5;
const spacing = 1;
const wordCols = word.length * letterWidth + (word.length - 1) * spacing;
const startCol = Math.floor((cols - wordCols) / 2);

function rect(col, row, fill) {
  const x = left + col * step;
  const y = top + row * step;
  return `<rect x="${x}" y="${y}" width="${cell}" height="${cell}" rx="2" fill="${fill}"/>`;
}

const background = [];
for (let row = 0; row < rows; row++) {
  for (let col = 0; col < cols; col++) {
    background.push(rect(col, row, empty));
  }
}

const foreground = [];
let cursor = startCol;
for (const char of word) {
  const pattern = letters[char];
  pattern.forEach((line, row) => {
    [...line].forEach((pixel, col) => {
      if (pixel === "1") {
        const shade = greens[(row + col + cursor) % greens.length];
        foreground.push(rect(cursor + col, row, shade));
      }
    });
  });
  cursor += letterWidth + spacing;
}

const svg = `<svg width="${width}" height="${height}" viewBox="0 0 ${width} ${height}" xmlns="http://www.w3.org/2000/svg" role="img" aria-labelledby="title desc">
  <title id="title">yunbom GitHub contribution style wordmark</title>
  <desc id="desc">The word yunbom drawn with GitHub contribution graph squares in green.</desc>
  <g>
    ${background.join("\n    ")}
  </g>
  <g>
    ${foreground.join("\n    ")}
  </g>
</svg>
`;

const outputPath = path.join(__dirname, "..", "outputs", "yunbom-github-green.svg");
fs.writeFileSync(outputPath, svg, "utf8");
console.log(outputPath);
