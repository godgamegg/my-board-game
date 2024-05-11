<script>
  let ground = [];
  let arr = [];
  for (let i = 0; i < 15; i++) {
    arr.push([]);
    ground.push([]);
    for (let j = 0; j < 15; j++) {
      ground[i].push(true);
      arr[i].push(-1);
    }
  }

  //턴, 돌 놓기
  //흑 = 홀수 백 = 짝수
  let turn = 0;
  let gameover = false;
  let way = [
    [1, 1],
    [0, 1],
    [-1, 1],
    [-1, 0],
  ];
  const stack = (x, y) => {
    if (ground[y][x] == true && gameover != true) {
      ground[y][x] = false;
      turn += 1;
      turn %= 2;
      arr[y][x] = turn;
      let check = [0, 0, 0, 0];
      let point;
      let stack;
      for (let i = 0; i < 4; i++) {
        stack = 0;
        point = arr[y][x];
        for (let j = 0; j < 5; j++) {
          if (
            y + j * way[i][0] < 15 &&
            x + j * way[i][1] < 15 &&
            y + j * way[i][0] > -1 &&
            x + j * way[i][1] > -1
          ) {
            console.log(y + j * way[i][0], x + j * way[i][1]);
            if (point == arr[y + j * way[i][0]][x + j * way[i][1]]) {
              stack += 1;
            } else {
              break;
            }
          } else {
            break;
          }
        }
        for (let j = 0; j < 5; j++) {
          if (
            y - j * way[i][0] > -1 &&
            x - j * way[i][1] > -1 &&
            y - j * way[i][0] < 15 &&
            x - j * way[i][1] < 15
          ) {
            console.log(y - j * way[i][0], x - j * way[i][1]);
            if (point == arr[y - j * way[i][0]][x - j * way[i][1]]) {
              stack += 1;
            } else {
              break;
            }
          } else {
            break;
          }
        }
        // console.log(stack);
        if (stack == 6) {
          if (point == 0) {
            alert("흰색 승리");
            gameover = true;
          } else if (point == 1) {
            alert("검은색 승리");
            gameover = true;
          }
        }
      }
      //   console.log(arr);
    }
  };
</script>

<table>
  <tbody>
    {#each arr as row, y}
      <tr>
        {#each row as is_mine, x}
          <td on:click={() => stack(x, y)}
            >{#if is_mine === 0}
              <div class="placed white"></div>
            {:else if is_mine === 1}
              <div class="placed black"></div>
            {:else if is_mine === -1}
              x{x}y{y}
            {/if}
          </td>
        {/each}
      </tr>
    {/each}
  </tbody>
</table>

<style>
  table {
    border-collapse: collapse;
  }
  td {
    width: 50px;
    height: 50px;
    border: 2px solid black;
    background-color: lightgray;
    text-align: center;
    transition: all 0.3s;
    font-size: 10px;
  }
  td:hover {
    filter: brightness(0.7);
    cursor: pointer;
  }
  div.placed.white {
    width: 100%;
    height: 100%;
    background-color: white;
    border-radius: 50%;
  }
  div.placed.black {
    width: 100%;
    height: 100%;
    background-color: black;
    border-radius: 50%;
  }
</style>
