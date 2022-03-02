---
id: JdoBprnhKfFmz54NRfga3
title: React
desc: ''
updated: 1644859154457
created: 1644824758017
---

React is a JavaScript Library for building user interfaces.
https://reactjs.org/tutorial/tutorial.html
https://codepen.io/gaearon/pen/oWWQNa?editors=0011
tic-tac-toe game
```js
/**
class Square extends React.Component {  // a controlled components. will not do any action
  render() {
    return (
      <button 
        className="square"
        onClick={() => {
          this.props.onClick(); // this send the function from parent (Board) to Square. props is passed as Component's parameter
          // this.setState({value: 'X'});  // this set the state object
          console.log('click-' + this.props.value);
        }}
      >
        {this.props.value}
      </button> // this value is what will be shown on the board
    );
  }
}
*/

function Square(props) {  // a function component does not extend React.Component. Only need the input props param
  return (
    <button
      className="square"
      onClick={props.onClick} // both () are removed
    >
      {props.value}
    </button> // no longer have this
  );  
}

class Board extends React.Component {
  constructor(props) { // make a state to store the value of this component
    super(props);
    this.state = {  // a component has props, and now we add 'state' field. We put 'value' to 'state' object
      squares: Array(9).fill(null),
      xIsNext: true,
    }; // the state is stored in Board instead of individual Square components
  }
  
  handleClick(i) {
    const squares = this.state.squares.slice(); // make a shallow copy of squares array to modify instead of the existing array
    if (calculateWinner(squares) || squares[i]) { // ignore click after winner or board is filled
      return;
    }
    squares[i] = this.state.xIsNext? 'X' : 'O'; // on that index, it changes from null to 'X'
    this.setState({
      squares: squares,
      xIsNext: !this.state.xIsNext,
    });  // update the state of the board. And will re-render Square's components
  }

  renderSquare(i) {
    return (
      <Square
        value={this.state.squares[i]} // = Square.props.value
        onClick={() => this.handleClick(i)}
      />
    );
  }

  render() {
    const winner = calculateWinner(this.state.squares);
    let status;
    if (winner) { // if there is winner
      status = "Winner is " + winner;
    } else {
      status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
    }

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}

// ========================================

ReactDOM.render(
  <Game />,
  document.getElementById('root')
);

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```