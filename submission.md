# สรุป

- ตอนที่ 1 เครื่องมือจำเป็น<br />

  - เครื่องมือที่ต้องใช้ในการทดลองนี้มือ
    Chocolatey ใช้สำหรับติดตั้ง package ที่จำเป็นอื่น ๆ ได้สะดวกมากยิ่งขึ้น
  - Node.js เป็นตัว compile โค้ดที่เราเขียนขึ้น
  - Yarn คือตัวจัดการ package ของ Javascript
  - Git ใช้สำหรับจัดการ Version ของ Code ที่เขียนขึ้น เหมาะสมสำหรับบริการจัดการโปรแกรมที่ร่วมกันเขียนได้คน
  - create-react-app CLI ใช้ในการสร้าง package module ทั้งหมดที่ React ต้องการใช้
  - Visual Studio Code ใช้สำหรับเป็น IDE ในการเขียนโปรแกรม

- ตอนที่ 2 Checkpoint <br />
  การทดลองนี้มีการสร้าง Repository ไว้ใน github.com โดยลิงค์คือ https://github.com/PYKSOJ/3SA03.git <br />
- ตอนที่ 3 <br />
  คำสั่ง `create-react-app card-game` คือคำสั่งสร้างโปรเจค React ชื่อ cardgame<br />
  `cd card-game` คือคำสั่งเข้าถึง Directory ชื่อ card-game<br />
  `yarn start` คือคำสั่งเริ่มต้นการทำงานของโปรเจค card-game<br />
  มีการปรับปรุงที่ไฟล์ App.js ดังนี้

```import React from 'react';
import './App.css';
function App() {
  return (
    <div>Hello World</div>
  );
}
export default App;
```

จะมีข้อความแสดงว่า Hello World<br />
### JSX <br /><br/>
ทดลองเปลี่ยน ข้อความ Hello World ในไฟล์ App.js เป็น `Hello {“World”}` <br />

- ตอนที่ 4 Components & Props <br/>
  สร้างไฟล์ CharacterCard.js เพื่อกำหนดคุณสมบัติคอมโพเนนต์ CharacterCard

```
import React from 'react';
export default function CharacterCard(props) {
  return (
    <div>{props.value}</div> 
  ) 
}
```
แก้ไขไฟล์ App.js
```
import CharacterCard from './CharacterCard';
function App() {
  return ( 
    <div> 
      <CharacterCard value="h"/>
      <CharacterCard value="i"/> 
    </div> 
  );
}
```
App.js จะดึงคุณสมบัติของ CharracterCard เข้ามาโดยใช้ value ในการกำหนดคุณสมบัติของแต่ละแท็กด้วย <br/>
แก้ไขไฟล์ App.js 

```
const word = "Hello";
function App() {
   return ( 
    <div> 
    {
      Array.from(word).map((c, i) => <CharacterCard value={c} key={i}/>) 
    } 
    </div> 
  ); 
}
```
การใส่ Props ชื่อว่า Key จะช่วยอ้างอิงในกรณีวนซ้าเพื่อสร้างคอมโพเนนต์
<br/><br/>
 ### Styling
<br/>
เพิ่ม CSS Class ชื่อ card ในไฟล์ App.css<br/>

```
.card {
    display: inline-block;
    text-align: center;
    width: 3em;
    font-size: 2em;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    margin: 1em;
    user-select: none;
}
```
เป็นการตกแต่งให้ดูสวยมากมากยิ่งขึ้น<br/>
### More
เพิ่มไฟล์ WordCard.js
```
import React from 'react';
export default function WordCard(props){
  return (
    <div>
      { Array.from(props.value).map((c, i) => 
        <CharacterCard value={c} key={i}/>) 
      }
    </div>
  );
}
```
ปรับเปลี่ยนไฟล์ App.js ให้ทาการ render คอมโพเนนต์ WordCard แทนที่จะ render คอมโพเนนต์ CharacterCard
โดยตรง<br/>
แก้ไข App.js 
```
return (
  <div>
    <WordCard value="hello"/>
  </div>
);
```

- ตอนที่ 5 Components & States <br/>
  ปรับปรุง CharacterCard ให้รองรับการถูกคลิก
```
import React, { useState } from 'react';
export default function CharacterCard(props) {    
  const [active, setActive] = useState(false); 
  const activate = () => { setActive(true) } 
  const className = `card ${active ? 'activeCard': ''}` 
  return (
    <div className={className} onClick={activate}>
    {props.value}</div> 
  ) 
}
```
ปรับปรุงไฟล์ App.css
```
.activeCard { 
  color: red; 
  background: pink; 
}
```
### Handler as Props
แก้ไขไฟล์ WordCard.js จานวนสองจุด คือการนิยามเมธอดในฟังก์ชัน WordCard และการส่งผ่าน handler ไปเป็น props
```
const activationHandler = c => { 
  console.log(`${c} has been activated.`) 
  } 
  ... 
  <CharacterCard value={c} key={i} activationHandler={activationHandler}/>

```
แก้ไขไฟล์ CharacterCard.js ให้เรียกใช้ activationHandler
```
const activate = () => { 
  if(!active){ 
    setActive(true) props.activationHandler(props.value) 
  } 
}
```
- ตอนที่ 6 Game Logic<br/>
ติดตั้งไลบรารี lodash
```
yarn add lodash
```
ปรับปรุงไฟล์ WordCard.js เพื่อให้ทาการสุ่มลาดับของตัวอักษรในคาที่จะแสดงผล
```
import _ from 'lodash';
const prepareStateFromWord = (given_word) => { 
  let word = given_word.toUpperCase() let chars =_.shuffle(Array.from(word)) 
  return { 
    word, 
    chars, 
    attempt: 1, 
    guess: '', 
    completed: false 
  } 
}
```
```
const activationHandler = (c) => { 
  console.log(`${c} has been activated.`) 
  let guess = state.guess + c setState({
    ...state, guess
  }) 
  if(guess.length == state.word.length){
    if(guess == state.word){ 
      console.log('yeah!') setState({...state, guess: '',
      completed: true}) 
    }else{ console.log('reset') setState({...state, guess: '', attempt: state.attempt + 1}) 
    }
  }
```
แก้ไขไฟล์ CharacterCard.js เพื่อตรวจสอบว่า เมื่อ attempt
```
const attemptRef = useRef(props.attempt); 
                ... 
useEffect(() => { 
  if(attemptRef.current != props.attempt){ 
    setActive(false) attemptRef.current = props.attempt 
  } 
})
```
- ตอนที่ 7 <br/>
  Commit ที่ 1 ทดลองใส่ Font 
```
  yarn add @fortawesome/fontawesome-svg-core
```
แต่ไม่สามารถหาวิธีในการทำได้<br/>
  Commit ที่ 2 cofig .gitignore เนื่องจากมีการเก็บไฟล์อื่น ๆ ที่เกี่ยวข้องเช่นไฟล์แล็ปชีทไว้ด้วยทพให้ต้องต้องมีการยกเว้นไฟล์บางประเภท<br/>
  commit ที่ 3 ศึกษาวิธีการสร้างเกม OX ขึ้น <br/>
  commit ที่ 4 เขียน Readme.md เพื่อให้อ่านรายละเอียดคร่าวๆของ Repository นี้<br/>
  commit ที่ 5 เขียน submission.md เพื่อสรุปผลจากการทำแล็ป 3SA03<br/>