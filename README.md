# Data Visualization
For this assignment, I wanted to put together a data visualization that read from this JSON file: 
https://raw.githubusercontent.com/prust/wikipedia-movie-data/master/movies.json

Specifically, I wanted it to isolate horror movies (from the generes). Then I wanted to isolate the horror movies by year from 1980 to 1999.

This then allowed me to display the # of horror movies released by year, by using vertical strokes and text to get a quick, at-a-glance sense of the data involved. I think it works well! It appears a bit simple, but I actually think that is a good thing to not detract from the data itself (in this particular case).

## Code:
```
let test;
let horrorCounts = [];

//Using the preload function to get the JSON file to load before the rest of the functions
function preload() {
  test = loadJSON('https://raw.githubusercontent.com/prust/wikipedia-movie-data/master/movies.json', (data) => {
    if (data && Array.isArray(data)) {
//Map horror genre + years
      let horrorMap = {}
      data.forEach(movie => {
        if (movie.genres.includes("Horror")) {
          let year = movie.year
          if (year >= 1980 && year <= 1999) { 
            horrorMap[year] = (horrorMap[year] || 0) + 1
          }
        }
      });
      horrorCounts = Object.entries(horrorMap).map(([year, count]) => ({ year: parseInt(year), count }))
    }
  })
}

function setup() {
  createCanvas(1200, 600)
  background(220)
  
//Add title to Canvas
  textSize(32)
  textFont('Helvetica-Light') 
  textStyle(BOLD)
  textAlign(CENTER)
  fill(0)
  text("# OF HORROR FILMS IN AMERICAN CINEMA FROM 1980 TO 1999", width / 2, 40)
}

//Check for data availability
function draw() {
  if (horrorCounts.length === 0) return

  textSize(32)
  textFont('Helvetica-Light')
  textStyle(NORMAL)

//Set bar size and distribute across width of canvas
  let barWidth = width / horrorCounts.length

  for (let i = 0; i < horrorCounts.length; i++) {
    let x = i * barWidth + barWidth / 2
    let y = height - 50
// Height scaling
    let h = -horrorCounts[i].count * 8

    stroke(0);
    strokeWeight(2);
    line(x, y, x, y + h)
    fill(0)

// Remove first two digits of the year and replace with with "'"
    let shortYear = "'" + String(horrorCounts[i].year).slice(2)
    textAlign(CENTER)
    text(shortYear, x, height - 10)
// Display movie count above the stroke
    text(horrorCounts[i].count, x, y + h - 10)
  }

  noLoop()
}

```

## p5 Sketch Link:
https://editor.p5js.org/rsutter/full/llwXjHgxG
