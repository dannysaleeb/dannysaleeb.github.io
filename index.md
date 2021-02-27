<html lang="en" style="height: 100%;">
    <head>
        <title>Reading Rhythms 2</title>
    </head>
    <body style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100%;">
        <div id="exercise-container-one" style="display: flex; margin-bottom: 20px;"></div>
    </body>
    <script>

        // create array containing all image filenames
        let imageFiles = [];
        for (let i = 0; i < 42; i++) {
            imageFiles.push(`img/tile_${i}.png`);
        }

        class Tile {

            constructor(image, width, height, startTie, endTie, text, colours) {
                this.image = image;
                this.width = width;
                this.height = height;
                this.startTie = startTie;
                this.endTie = endTie;
                this.text = text;
                this.colours = colours;
            }

            render = (target) => {

                let element = document.createElement('div');
                element.style.backgroundImage = `url(${this.image})`;
                element.style.backgroundSize = `${this.width} ${this.height}`;
                element.style.width = this.width;
                element.style.height = this.height;
                element.innerHTML = this.text;
                element.style.backgroundColor = this.colours[this.text % 3];
                element.style.display = 'flex';
                element.style.justifyContent = 'center';
                element.style.alignItems = 'center';
                target.appendChild(element);

            }

        }

        let tiles = {
            twoTwo: [],
            twoFour: [],
            twoEight: []
        }

        // populate twoTwo array in tiles object
        for (let i = 0; i < 14; i++) {
            tiles.twoTwo.push(new Tile(imageFiles[i], '140px', '140px', false, false, '', ['#ff0000', '#00ff00', '#0000ff']));
        }

        // populate twoFour array in tiles object
        for (let i = 14; i < 28; i++) {
            tiles.twoFour.push(new Tile(imageFiles[i], '140px', '140px', false, false, '', ['#ff0000', '#00ff00', '#0000ff']));
        }

        // populate twoEight array in tiles object
        for (let i = 28; i < 42; i++) {
            tiles.twoEight.push(new Tile(imageFiles[i], '140px', '140px', false, false, '', ['#ff0000', '#00ff00', '#0000ff']));
        }

        // two arrays to index tiles that have tie falling on start note and those that end with a tie going out of the tile
        let endTie = [5,6,7,11,12]
        let startTie = [8,9,10,11,12]

        // set relevant start tie values to true
        for (let i = 0; i < startTie.length; i++) {
            tiles.twoTwo[startTie[i]].startTie = true;
            tiles.twoFour[startTie[i]].startTie = true;
            tiles.twoEight[startTie[i]].startTie = true;
        }

        // set relevant end tie values to true
        for (let i = 0; i < endTie.length; i++) {
            tiles.twoTwo[endTie[i]].endTie = true;
            tiles.twoFour[endTie[i]].endTie = true;
            tiles.twoEight[endTie[i]].endTie = true;
        }

        // get keys for arrays in tiles object
        let tilesArrays = Object.keys(tiles);

        // get random index to choose array from tiles object
        let timeSigIndex = Math.floor(Math.random() * tilesArrays.length);

        // Get six tiles from single array, keeping time sig in position index 0 -- pass in array from tiles object
        function getSix(tiles) {
            let finalArray = [tiles[0]];
            let endTie = false;
            for (let i = 0; i < 6; i++) {
                // get a random index between 0 and length of tiles array
                let randomIndex = Math.floor(Math.random() * tiles.length);
                if (i === 5 && endTie === true) {
                    while (finalArray.includes(tiles[randomIndex]) || tiles[randomIndex].endTie === true || tiles[randomIndex].startTie === false) {
                        randomIndex = Math.floor(Math.random() * tiles.length);
                    }
                    finalArray[i+1] = tiles[randomIndex];
                } else if (i === 5 && endTie === false) {
                    while (finalArray.includes(tiles[randomIndex]) || tiles[randomIndex].endTie === true || tiles[randomIndex].startTie === true) {
                        randomIndex = Math.floor(Math.random() * tiles.length);
                    }
                    finalArray[i+1] = tiles[randomIndex];
                } else if (endTie === false) {
                    // while finalArray already includes the tile at that random index, keep getting random indices
                    while (finalArray.includes(tiles[randomIndex]) || tiles[randomIndex].startTie === true) {
                        randomIndex = Math.floor(Math.random() * tiles.length);
                    }
                    finalArray[i+1] = tiles[randomIndex];
                    if (tiles[randomIndex].endTie === true) {
                        endTie = true;
                    }
                } else {
                    while (finalArray.includes(tiles[randomIndex]) || tiles[randomIndex].startTie === false) {
                        randomIndex = Math.floor(Math.random() * tiles.length);
                    }
                    finalArray[i+1] = tiles[randomIndex];
                    if (tiles[randomIndex].endTie === false) {
                        endTie = false;
                    }
                }
            }

            return finalArray;
        }


        let chosenTiles = getSix(tiles[tilesArrays[timeSigIndex]]);

        for (let i = 0; i < 7; i++) {
            chosenTiles[i].render(document.querySelector('#exercise-container-one'));
        }

    </script>
</html>
