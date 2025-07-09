# mineSweeper_JS

{

    let instagram_button_div = document.getElementsByClassName("instagram_button_div")[0]
    let instagram_button_img1 = document.getElementsByClassName("instagram_button_img1")[0]
    let instagram_button_img2 = document.getElementsByClassName("instagram_button_img2")[0]
    let youtube_button_div = document.getElementsByClassName("youtube_button_div")[0]
    let youtube_button_img1 = document.getElementsByClassName("youtube_button_img1")[0]
    let youtube_button_img2 = document.getElementsByClassName("youtube_button_img2")[0]
    
    instagram_button_img1.style.transition = "opacity 200ms";
    instagram_button_img2.style.transition = "opacity 200ms";
    youtube_button_img1.style.transition = "opacity 200ms";
    youtube_button_img2.style.transition = "opacity 200ms";
    
    instagram_button_div.addEventListener("mouseover", function(){
       instagram_button_img1.style.opacity = '0';
       instagram_button_img2.style.opacity = '1';
    });
    instagram_button_div.addEventListener("mouseout", function(){
       instagram_button_img1.style.opacity = '1';
       instagram_button_img2.style.opacity = '0';
    });
    youtube_button_div.addEventListener("mouseover", function(){
       youtube_button_img1.style.opacity = '0';
       youtube_button_img2.style.opacity = '1';
    });
    youtube_button_div.addEventListener("mouseout", function(){
       youtube_button_img1.style.opacity = '1';
       youtube_button_img2.style.opacity = '0';
    });
    
    instagram_button_div.addEventListener("click", function(){
       window.open("https://www.instagram.com/thomaz_j7/", "_blank");
    });
    youtube_button_div.addEventListener("click", function(){
       window.open("https://www.youtube.com/@thomazsoares1316/playlists", "_blank");
    });
    
    
    
    function isValid_index(i, j, arrayLen_i, arrayLen_j){
       if (i >= 0 && j >= 0 && i < arrayLen_i && j < arrayLen_j){
          return (1);
       } else{
          return (0);
       }
    }
    
    function validIndexes(i, j){
       let valid_indexes = -1;
       
       for (let i1 = i - 1; i1 <= i + 1; i1++){
          for (let j1 = j - 1; j1 <= j + 1; j1++){
             if (isValid_index(i1, j1, rows, cols)){
                valid_indexes++;
             }
          }
       }
    
       return (valid_indexes);
    }
    
    function bombCount(i, j, grid){
       let bomb1_count = 0;
       
       for (let i1 = i - 1; i1 <= i + 1; i1++){
          for (let j1 = j - 1; j1 <= j + 1; j1++){
             if (isValid_index(i1, j1, rows, cols)){
                if (grid[i1][j1] == 1){
                   bomb1_count++;
                }
             }
          }
       }
    
       return (bomb1_count);
    } /* counts the middle one if true */
    
    
    const rows = 20;
    const cols = 32;
    
    function mineSweeper(){
       let grid = Array(rows).fill(0).map(() => Array(cols).fill(0));
    
       let num_rand1;
       for (let i = 0; i < rows; i++){
          for (let j = 0; j < cols; j++){
             num_rand1 = Math.floor(Math.random() * 10) + 1;
    
             if (num_rand1 <= 5){
                grid[i][j] = 1;
             }
          }
       }
    
       for (let i = 0; i < rows; i++){
          for (let j = 0; j < cols; j++){
             if (grid[i][j] == 1){
                if (bombCount(i, j, grid) == 9){
                   grid[i - 1][j] = 2;
                   grid[i][j - 1] = 2;
                   grid[i][j + 1] = 2;
                   grid[i + 1][j] = 2;
                } else if (validIndexes(i, j) == 5 && bombCount(i, j, grid) == 6){
                   if (isValid_index(i - 1, j, rows, cols)){grid[i - 1][j] = 2;}
                   if (isValid_index(i, j - 1, rows, cols)){grid[i][j - 1] = 2;}
                   if (isValid_index(i, j + 1, rows, cols)){grid[i][j + 1] = 2;}
                   if (isValid_index(i + 1, j, rows, cols)){grid[i + 1][j] = 2;}
                } else if (validIndexes(i, j) == 3 && bombCount(i, j, grid) == 4){
                   if (isValid_index(i - 1, j, rows, cols)){grid[i - 1][j] = 2;}
                   if (isValid_index(i, j - 1, rows, cols)){grid[i][j - 1] = 2;}
                   if (isValid_index(i, j + 1, rows, cols)){grid[i][j + 1] = 2;}
                   if (isValid_index(i + 1, j, rows, cols)){grid[i + 1][j] = 2;}
                }
             }
          }
       }
       
       for (let i =  0; i < rows; i++){
          for (let j = 0; j < cols; j++){
             if (grid[i][j] == 0){
                let bomb1_countTotal;
                if (validIndexes(i, j) == 8){
                   bomb1_countTotal = 5;
                } else if (validIndexes(i, j) == 5){
                   bomb1_countTotal = 2;
                } else{
                   bomb1_countTotal = 3;
                }
    
                let bomb1_count = bombCount(i, j, grid);
                      
                let inc1 = 1;
                if (bombCount(i, j, grid) >= bomb1_countTotal){
                   while ((isValid_index(i, j - inc1, rows, cols) || isValid_index(i, j + inc1, rows, cols)) && bomb1_count >= bomb1_countTotal){
                      if (validIndexes(i, j) == 8){
                         bomb1_countTotal = 6;
                      } else if (validIndexes(i, j) == 5){
                         bomb1_countTotal = 4;
                      } else{
                         bomb1_countTotal = 5;
                      }
                      
                      grid[i][j - inc1] = 3;
                      grid[i][j + inc1] = 3;
    
                      bomb1_count = bombCount(i, j - inc1, grid);
    
                      inc1++;
                   }
                }
             }
          }
       }
    
       /*# 2, 3 --> 0 #*/
    
       let b1 = 0;
       let b2 = 0;
       let b3 = 0;
       let b4 = 0;
       for (let i = 0; i < rows; i++){
          for (let j = 0; j < cols; j++){
             if (grid[i][j] == 0){
                b1++;
             } else if (grid[i][j] == 1){
                b2++;
             } else if (grid[i][j] == 2){
                b3++;
             } else{
                b4++;
             }
          }
       }
    
       console.log("-----");
       console.log(b1 + b3 + b4);
       console.log(b2);
       console.log((b1 + b3 + b4) / b2)
       console.log("-----");
    
       return grid;
    }
    
    
    
    let game_grid = document.getElementsByClassName("game_grid")[0];
    
    const efg = mineSweeper();
    let k = 0;
    let sub_gridArray = [];
    let sub_gridTypeArray = [];
    let bomb1_imageArray = [];
    for (let i = 0; i < rows; i++){
       for (let j = 0; j < cols; j++){
          let sub_grid = document.createElement("sub_grid");
          game_grid.appendChild(sub_grid);
    
          sub_grid.style.alignContent = "center";
          sub_grid.style.textAlign = "center";
          sub_grid.style.fontFamily = "sans-serif";
          sub_grid.style.fontSize = "24px";
          sub_grid.style.border = "black 1px solid";
          sub_grid.style.cursor = "pointer";
    
          if (efg[i][j] == 1){
             sub_grid.style.backgroundColor = "red"; /* mine */
             sub_gridTypeArray.push(1);
    
             let bomb1_image = new Image;
             bomb1_image.src = "images/bomb1.png";
             bomb1_image.style.height = "0px";
             bomb1_image.style.width = "0px";
             sub_grid.appendChild(bomb1_image);
             bomb1_imageArray.push(bomb1_image);
          } else{
             sub_grid.style.backgroundColor = "lime"; /* not mine */
             sub_gridTypeArray.push(0);
          }
    
          sub_gridArray.push(sub_grid);
       }
    }
    
    function loseGame(q){
       console.log(`You lost! 
       
       ${q} squares mined!`);
    
       let k = 0;
       for (let i = 0; i < rows * cols; i++){
          if (sub_gridTypeArray[i] == 1){
             sub_gridArray[i].style.backgroundColor = "tomato";
             bomb1_imageArray[k].style.height = "20px";
             bomb1_imageArray[k].style.width = "20px";
    
             k++;
          }
       }
    }
    
    let notMines = 0;
    let continueGame = 1;
    let SGArray = [];
    /* toCopy >>> SG.target */
    game_grid.addEventListener('click', (SG) => {
       if (continueGame && !(SGArray.includes(SG.target))){
          SGArray.push(SG.target);
          let SBI = sub_gridArray.indexOf(SG.target);
          SG.target.style.backgroundColor = "black";
          
          if (sub_gridTypeArray[SBI] == 1){
             loseGame(notMines);
             continueGame = 0;
          } else{
             notMines++;
             console.log(`Mines: ${notMines}`);
          }
    
       }
    });

}
