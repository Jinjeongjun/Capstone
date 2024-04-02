## State
# <버튼 클릭 시 +1 되는 기능>
<!DOCTYPE html>
<html>
  <body>
    <div id="root"></div>
  </body>
  <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
  <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
    const root = document.getElementById('root');
    function App() {
      const [counter, setCounter] = React.useState(0);
      const onClick = () => {
        // setCounter(counter + 1);
        setCounter((current) => current + 1);
      };
      return (
        <div>
          <h3>Total clicks: {counter}</h3>
          <button onClick={onClick}>Click me</button>
        </div>
      );
    }
    ReactDOM.render(<App />, root);
  </script>
</html>

# <단위 변환 (hours - minutes, km - miles)>
<!DOCTYPE html>
<html>
  <body>
    <div id="root"></div>
  </body>
  <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
  <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
    function MinutesToHours() {
      const [amount, setAmount] = React.useState(0);
      const [inverted, setInverted] = React.useState(false);
      const onChange = (event) => {
        setAmount(event.target.value);
      };
      const reset = () => setAmount(0);
      const onInvert = () => {
        reset();
        setInverted((current) => !current);
      };
      return (
        <div>
          <div>
            <label htmlFor="minutes">Minutes</label>
            <input
              value={inverted ? amount * 60 : amount}
              id="minutes"
              placeholder="Minutes"
              type="number"
              onChange={onChange}
              disabled={inverted}
            />
          </div>
          <div>
            <label htmlFor="hours">Hours</label>
            <input
              value={inverted ? amount : Math.round(amount / 60)}
              id="hours"
              placeholder="Hours"
              type="number"
              onChange={onChange}
              disabled={!inverted}
            />
          </div>
          <button onClick={reset}>Reset</button>
          <button onClick={onInvert}>
            {inverted ? "Turn back" : "Invert"}
          </button>
        </div>
      );
    }
    function KmToMiles() {
      const [distance, setDistance] = React.useState(0);
      const [flipped, setFlipped] = React.useState(false);
      const onChange = (event) => {
        setDistance(event.target.value);
      };
      const reset = () => setDistance(0);
      const onFlip = () => {
        reset();
        setFlipped((current) => !current);
      };
      return (
        <div>
          <div>
            <label htmlFor="km">KM</label>
            <input
              value={flipped ? Math.round(distance * 1.609344) : distance}
              id="km"
              placeholder="KM"
              type="number"
              onChange={onChange}
              disabled={flipped}
            />
          </div>
          <div>
            <label htmlFor="miles">Miles</label>
            <input
              value={flipped ? distance : Math.round(distance * 0.621371)}
              id="miles"
              placeholder="Miles"
              type="number"
              onChange={onChange}
              disabled={!flipped}
            />
          </div>
          <button onClick={reset}>Reset</button>
          <button onClick={onFlip}>{flipped ? "Turn back" : "Flip"}</button>
        </div>
      );
    }
    function App() {
      const [index, setIndex] = React.useState("XX");
      const onSelect = (event) => {
        setIndex(event.target.value);
      };
      return (
        <div>
          <h1>Super Converter</h1>
          <select value={index} onChange={onSelect}>
            <option value="XX">Select your units</option>
            <option value="0">Minutes & Hours</option>
            <option value="1">Km & Miles</option>
          </select>
          <hr />
          {index === "XX" ? "Please select your units" : null}
          {index === "0" ? <MinutesToHours /> : null}
          {index === "1" ? <KmToMiles /> : null}
          {/* 2개일 때는 이거도 가능?
        {index === "0" ? <MinutesToHours /> : <KmToMiles />}*/}
          {index === "0" ? <MinutesToHours /> : <KmToMiles />}*/}
        </div>
      );
    }
    const root = document.getElementById("root");
    ReactDOM.render(<App />, root);
  </script>
</html>

## Props
<!DOCTYPE html>
<html>
  <body>
    <div id="root"></div>
  </body>
  <script src="https://unpkg.com/react@17.0.2/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
  <script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
    function Btn({ text, fontSize = 14 }) {
      return (
        <button
          style={{
            backgroundColor: 'tomato',
            color: 'white',
            padding: '10px 20px',
            border: '0',
            borderRadius: 10,
            fontSize,
          }}
        >
          {text}
        </button>
      );
    }
    Btn.propTypes = {
      text: PropTypes.string.isRequired,
      fontSize: PropTypes.number,
    };
    function App() {
      return (
        <div>
          <Btn text="Save Changes" fontSize={18} />
          <Btn text="Continue" />
        </div>
      );
    }

    const root = document.getElementById('root');
    ReactDOM.render(<App />, root);
  </script>
</html>
 

## Effects
# <useEffect 기능 사용법>
import { useState, useEffect } from 'react';

function App() {
  const [counter, setValue] = useState(0);
  const [keyword, setKeyword] = useState('');
  const onClick = () => setValue((prev) => prev + 1);
  const onChange = (event) => {
    setKeyword(event.target.value);
  };
  useEffect(() => {
    console.log('I run only once');
  }, []); // 처음 실행될 때만 작동
  useEffect(() => {
    console.log("I run when 'keyword' changes.");
  }, [keyword]); // keyword가 변경될 때 작동
  useEffect(() => {
    console.log("I run when 'counter' changes.");
  }, [counter]); // counter가 변경될 때 작동(click me 버튼 눌렀을 때)
  useEffect(() => {
    console.log('I run when keyword & counter changes.');
  }, [keyword, counter]); // keyword나 counter 값에 변화가 생겼을 때 작동

  return (
    <div>
      <input
        value={keyword}
        onChange={onChange}
        type="text"
        placeholder="Search here..."
      />
      <h1>{counter}</h1>
      <button onClick={onClick}>click me</button>
    </div>
  );
}

export default App;
