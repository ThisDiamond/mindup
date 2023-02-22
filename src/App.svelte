<script lang="ts">
  let num1: number;
  let num2: number;
  let answer: number;
  let ishora: string;
  let ishoraNum: number;
  let answerInput: string = "";

  // time
  let secondGame: number = 60;
  let timeGame: any;

  type Natija = {
    _num1: number;
    _num2: number;
    _ishora: number;
    _answer: number | undefined;
    _isRight: boolean;
  };
  let natija: Natija[] = [];

  let answerTrue: number = 0;
  let answerFalse: number = 0;

  //////////////////

  let mainWelcomeDisplay: string = "displayflex";
  let mainDisplay: string = "displaynone";
  let mainClockDisplay: string = "displaynone";
  let mainEndTimeDisplay: string = "displaynone";
  let mainMathDisplay: string = "displaynone";
  let mainExitButtonDisplay: string = "displaynone";

  let displayLogin: string = "displaynone";

  ////////////
  let check: string;
  let Music: string;
  let audio: any;

  function displayLoginForm() {
    if (displayLogin == "displaynone") displayLogin = "displayblock";
    else displayLogin = "displaynone";
  }

  function display() {
    mainDisplay = "displayflex";
    mainWelcomeDisplay = "displaynone";
    mainClockDisplay = "displayflex";
    mainMathDisplay = "displayflex";
    mainExitButtonDisplay = "displayflex";
  }

  function checkOperation(operation: string) {
    display();
    ishora = operation;
    randomNum();
    time();
  }

  function randomNum() {
    num1 = Math.floor(Math.random() * 10);
    num2 = Math.floor(Math.random() * 10);

    if (ishora == "+") {
      answer = num1 + num2;
      ishoraNum = 1;
    }
    if (ishora == "-") {
      answer = num1 - num2;
      ishoraNum = 2;
    }
    if (ishora == "*") {
      answer = num1 * num2;
      ishoraNum = 3;
    }
    if (ishora == "/") {
      answer = num1 / num2;
      ishoraNum = 4;
    }
  }

  // time

  function time() {
    timeGame = setInterval(() => {
      secondGame -= 1;
    }, 1000);
    setTimeout(() => {
      clearInterval(timeGame);
      mainDisplay = "displayflex";
      mainEndTimeDisplay = "displaynone";
      mainMathDisplay = "displaynone";
      mainEndTimeDisplay = "displayflex";
      if (mainWelcomeDisplay == "displayflex") {
        mainDisplay = "displaynone";
        mainEndTimeDisplay = "displaynone";
      }
    }, 60000);
  }

  function stopTime() {
    clearInterval(timeGame);
    secondGame = 60;
  }

  function submitEnter() {
    let bgcolorchange = setInterval(() => {
      check = "checkdefault";
    }, 500);
    setTimeout(() => {
      clearInterval(bgcolorchange);
    }, 500);

    let answerInput1 = Number(answerInput);
    if (answerInput1 == answer) {
      check = "checktrue";
      Music = "Music/good1.mp3";
      audio.play();
      // save arr
      natija.push({
        _num1: num1,
        _num2: num2,
        _ishora: ishoraNum,
        _answer: answerInput1,
        _isRight: true,
      });
      natija = natija;
      // generation new number
      randomNum();

      // the end
      answerTrue += 1;
    } else {
      check = "checkfalse";
      Music = "Music/bad3.mp3";
      audio.play();
      // save arr
      natija.push({
        _num1: num1,
        _num2: num2,
        _ishora: ishoraNum,
        _answer: answerInput1,
        _isRight: false,
      });
      natija = natija;
      // generation new number
      randomNum();
      // the end
      answerFalse += 1;
    }
    answerInput = "";
  }

  function exit() {
    mainWelcomeDisplay = "displayflex";
    mainDisplay = "displaynone";
    mainEndTimeDisplay = "displaynone";
    mainMathDisplay = "displaynone";
    mainExitButtonDisplay = "displaynone";
    mainClockDisplay = "displaynone";
    // clear arr
    natija = [];
    //time
    stopTime();
    // the end
    answerTrue = 0;
    answerFalse = 0;
  }
  // Music = "Music/children.mp3";
</script>

<body class={check}>
  <audio src={Music} autoplay bind:this={audio} />

  <header>
    <div class="logo">Mind<b>UP</b></div>
    <div class="clock {mainClockDisplay}">
      <!-- <span class="level"> Level - 1</span>  -->
      <i class="bi bi-alarm" /> Vaqt 00:{secondGame < 10
        ? "0" + secondGame
        : secondGame}
    </div>
    <div class="user">
      <i class="bi bi-box-arrow-in-right" />
      <button class="login" on:click={() => displayLoginForm()}> Kirish</button>
      <button class="register">Ro'yxatdan o'tish</button>
    </div>

    <div class="login-form {displayLogin}">
      <form action="">
        <h2><i class="bi bi-box-arrow-in-right" />Kirish</h2>
        <input type="text" placeholder="username" />
        <input type="text" placeholder="password" />
        <a href="localhost:8080">Parol esdan chiqdimi?</a>
        <button>submit</button>
      </form>
    </div>
  </header>

  <main>
    <div class="{mainWelcomeDisplay} mainWelcomeDisplay">
      <div class="tanlang"><b>Arifmetik</b> amalni tanlang</div>
      <div class="amallar">
        <button on:click={() => checkOperation("+")}>+</button>
        <button on:click={() => checkOperation("-")}>-</button>
        <button hidden on:click={() => checkOperation("*")}>*</button>
        <button hidden on:click={() => checkOperation("/")}>/</button>
      </div>
    </div>

    <!-- main -->
    <!-- left -->
    <div class="tekshirish {mainDisplay}">
      <!-- <h2>Natija</h2> -->
      <div class="tekshirish-overflow">
        <table>
          <tr>
            <th>No</th>
            <th>Misol</th>
            <th>Natija</th>
          </tr>
          {#each natija as natijaOut, i}
            <tr>
              <td>{i + 1}</td>
              <td
                >{natijaOut._num1}
                {natijaOut._ishora == 1
                  ? "+"
                  : natijaOut._ishora == 2
                  ? "-"
                  : natijaOut._ishora == 3
                  ? "*"
                  : "/"}
                {natijaOut._num2} = {natijaOut._answer}</td
              >
              <td
                ><i
                  class={natijaOut._isRight
                    ? "bi bi-check-square green"
                    : "bi bi-x-square red"}
                />
              </td>
            </tr>
          {/each}
        </table>
      </div>
    </div>

    <!-- center -->
    <div class="math  {mainMathDisplay}">
      <h1 id="num1">{num1}</h1>
      <h1 id="amal">{ishora}</h1>
      <h1 id="num2">{num2}</h1>
      <h1 id="">=</h1>
      <form on:submit|preventDefault={() => submitEnter()}>
        <input type="text" id="answerInput" bind:value={answerInput} />
      </form>
    </div>
    <div class="natija_endTime {mainEndTimeDisplay}">
      <h1 class="title_natija_endTime">TheEND</h1>
      <div>
        <h1 class="done">{answerTrue}</h1>
        <h1 class="notdone">{answerFalse}</h1>
      </div>
    </div>

    <!-- right -->
    <div class="right {mainDisplay}">Users</div>
    <!-- main end -->
  </main>

  <footer>
    <button class="exit {mainExitButtonDisplay}" on:click={() => exit()}>
      Chiqish
    </button>
  </footer>
</body>

<style>
  /* header */
  header {
    background-color: white;
    color: #3cb2da;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 25px;
  }
  .logo {
    font-size: 27px;
  }
  .user {
    display: flex;
    align-items: center;
  }
  .user i {
    margin-right: -5px;
    font-size: 24px;
  }
  .user .login {
    background-color: transparent;
    color: #3cb2da;
    text-transform: uppercase;
    font-weight: 600;
    padding: 8px 14px;
    border: 0;
    border-radius: 50px;
  }
  .user .register {
    background-color: #3cb2da;
    color: aliceblue;
    text-transform: uppercase;
    font-weight: 600;
    padding: 8px 14px;
    border: 0;
    border-radius: 50px;
  }
  .login-form {
    background-color: white;
    color: white;
    width: 250px;
    height: auto;
    padding: 15px;
    margin: 0;
    position: absolute;
    top: 70px;
    right: 25px;
    border-radius: 10px;
    box-sizing: border-box;
    box-shadow: rgba(64, 64, 64, 0.3) 0px 7px 20px 0px;
  }
  .login-form h2 {
    text-align: center;
    color: #3cb2da;
  }
  .login-form h2 i {
    margin-right: 10px;
  }
  .login-form input {
    background-color: aliceblue;
    width: 100%;
    height: 40px;
    margin: 10px 0;
    padding: 15px;
    font-size: 14px;
    border-radius: 50px;
    box-sizing: border-box;
  }
  .login-form a {
    color: #3cb2da;
    text-decoration: none;
    margin: 10px;
  }
  .login-form button {
    background-color: #3cb2da;
    color: white;
    width: 100%;
    padding: 12px;
    margin: 10px 0;
    border: 0;
    border-radius: 50px;
  }
  .clock {
    color: #f1635e;
    font-size: 27px;
    font-weight: 600;
  }
  .clock i {
    font-size: 27px;
    margin: 0 10px;
  }
  .level {
    font-size: 27px;
  }
  /* header  end*/
  /* main */

  .mainWelcomeDisplay {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 50px 0;
    margin: 0 auto;
  }

  .tanlang {
    text-align: center;
    text-transform: uppercase;
    font-size: 72px;
    transition: 500ms;
  }
  .tanlang:hover {
    font-size: 84px;
  }
  .amallar {
    display: flex;
    gap: 0 50px;
    font-size: 72px;
    transition: 500ms;
  }

  .amallar button {
    width: 150px;
    height: 150px;
    background-color: rgb(60, 178, 218);
    color: aliceblue;
    font-size: 112px;
    border: 0;
    border-radius: 10px;
  }
  .amallar:hover button:hover {
    background-color: rgb(48, 175, 217);
  }

  .right {
    background-color: #3cb2da;
    width: 250px;
    height: 75vh;
  }

  /* tekshirish */
  .tekshirish {
    width: 250px;
    height: 80vh;
    font-size: 120%;
  }
  /* .tekshirish h2 {
    text-align: center;
  } */
  .tekshirish-overflow {
    width: 100%;
    height: 100%;
    overflow-y: scroll;
  }
  ::-webkit-scrollbar {
    width: 10px;
  }
  ::-webkit-scrollbar-track {
    background-color: transparent;
  }
  /* korinishi
  ::-webkit-scrollbar-track{
    background-color: #f1f1f1;
    border-radius:10px;
  } */
  ::-webkit-scrollbar-thumb:hover {
    background-color: #cbcbcb;
  }

  /* tekshirish end */

  table {
    width: 100%;
    border-spacing: 3px;
  }
  td {
    min-width: 30px;
    border-collapse: collapse;
    text-align: center;
    padding: 3px;
    background-color: rgb(255, 255, 255);
  }

  .math {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 50px;
    font-size: 64px;
  }
  input {
    background-color: transparent;
    width: 180px;
    height: 150px;
    padding: 0;
    border: 0;
    outline: 0;
    font-weight: 700;
    font-size: 124px;
  }

  .natija_endTime {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    font-size: 36px;
  }
  .natija_endTime div {
    display: flex;
  }
  .natija_endTime h1 {
    min-width: 150px;
    max-width: fit-content;
    height: 150px;
    padding: 15px;
    margin: 0 15px;
    font-size: 112px;
    border: 0;
    border-radius: 10px;
  }
  .title_natija_endTime {
    font-size: 32px;
    color: black;
  }

  /* display*/
  .done {
    background-color: #a5db68;
    color: aliceblue;
  }
  .notdone {
    background-color: #fd6a63;
    color: aliceblue;
  }
  .displaynone {
    display: none;
  }

  .displayflex {
    display: flex;
  }
  .red {
    color: red;
  }
  .green {
    color: green;
  }
  /* exit button */
  .exit {
    float: right;
    height: 100%;
    background-color: #3cb2da;
    color: rgb(255, 255, 255);
    font-weight: 600;
    text-transform: uppercase;
    padding: 8px 12px;
    border: 0;
    transform: all 300ms 0s ease;
  }
  .exit:hover {
    background-color: rgb(40, 172, 216);
  }
  .exit:active {
    background-color: rgb(6, 155, 204);
  }
  .checktrue {
    background-color: #3cb2da;
  }
  .checkfalse {
    background-color: #f1635e;
  }
  .checkdefault {
    background-color: white;
  }
</style>
