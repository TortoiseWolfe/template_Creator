# React Component Generator Script with Bootstrapped Components

This script automates the creation of a master React component along with its subcomponents. It not only creates the directories and files but also bootstraps the `.tsx` files with basic React component code. The master component imports and renders the subcomponents.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [Usage](#usage)
- [Script Explanation](#script-explanation)
- [Example](#example)
- [Tips](#tips)

## Prerequisites

- **Node.js and npm**: Ensure you have Node.js and npm installed.
- **React Project Setup**: This script assumes you're working within an existing React project.
- **Git Bash**: Required if you're on Windows. macOS and Linux users can use the default terminal.
- **Basic Command Line Knowledge**: Familiarity with navigating directories and running scripts.

## Setup

1. **Navigate to Your Project's `src` Directory**

   Open your terminal and navigate to your project's `src` directory:

   ```bash
   cd /path/to/your/project/src
   ```

2. **Create the Script File**

   Create a new file named `create_components.sh`:

   ```bash
   touch create_components.sh
   ```

3. **Add the Script Content**

   Open `create_components.sh` in your preferred text editor and paste the following code:

   ```bash
   #!/bin/bash

   # Check if at least one component is provided
   if [ "$#" -lt 1 ]; then
     echo "Usage: $0 MasterComponent [SubComponent1 SubComponent2 ...]"
     exit 1
   fi

   # Get the master component name
   master_component="$1"
   shift

   # Create the master component directory
   if [ ! -d "$master_component" ]; then
     mkdir "$master_component"
     echo "Created master component directory '$master_component'."
   else
     echo "Master component directory '$master_component' already exists. Skipping creation."
   fi

   # Create the master component files with boilerplate code
   cat > "$master_component/$master_component.tsx" << EOF
   import React from 'react';
   $(for component in "$@"; do
     echo "import $component from './$component/$component';"
   done)

   const $master_component: React.FC = () => {
     return (
       <div className="$master_component">
         <h1>$master_component Component</h1>
   $(for component in "$@"; do
     echo "        <$component />"
   done)
       </div>
     );
   };

   export default $master_component;
   EOF

   # Create a basic CSS file for the master component
   echo ".$master_component { }" > "$master_component/$master_component.css"

   echo "Bootstrapped master component '$master_component'."

   # Create subcomponents inside the master component directory
   for component in "$@"; do
     if [ ! -d "$master_component/$component" ]; then
       mkdir "$master_component/$component"
       echo "Created subcomponent directory '$component' inside '$master_component'."

       # Create the subcomponent files with boilerplate code
       cat > "$master_component/$component/$component.tsx" << EOF
       import React from 'react';

       const $component: React.FC = () => {
         return (
           <div className="$component">
             <h2>$component Component</h2>
           </div>
         );
       };

       export default $component;
       EOF

       # Create a basic CSS file for the subcomponent
       echo ".$component { }" > "$master_component/$component/$component.css"

       echo "Bootstrapped subcomponent '$component'."
     else
       echo "Subcomponent directory '$component' already exists in '$master_component'. Skipping creation."
     fi
   done
   ```

4. **Make the Script Executable**

   In your terminal, run:

   ```bash
   chmod +x create_components.sh
   ```

## Usage

Run the script with the master component name followed by the names of the subcomponents you want to create. For example:

```bash
./create_components.sh Game Dice Player_Cards Score_Board
```

This command will:

- Create the `Game` master component.
- Create subcomponents `Dice`, `Player_Cards`, and `Score_Board`.
- Populate each `.tsx` file with React component boilerplate code.
- The master component imports and renders each subcomponent.

## Script Explanation

### Master Component Creation

- **Directory Creation**: Checks if the master component directory exists; if not, it creates it.
- **Boilerplate Code**: Writes a React functional component into the master `.tsx` file.
  - **Imports Subcomponents**: Dynamically imports each subcomponent.
  - **Renders Subcomponents**: Includes each subcomponent in the JSX.
- **CSS File**: Creates a basic `.css` file for the master component.

### Subcomponents Creation

- **Directory Creation**: For each subcomponent, checks if the directory exists; if not, it creates it.
- **Boilerplate Code**: Writes a React functional component into each subcomponent's `.tsx` file.
- **CSS File**: Creates a basic `.css` file for each subcomponent.

### Code Breakdown

- **Dynamic Imports and Rendering**:
  - The script uses `for` loops within `$(...)` to dynamically generate import statements and JSX elements for subcomponents.
  - **Example Import**:
    ```javascript
    import Dice from './Dice/Dice';
    ```
  - **Example JSX Rendering**:
    ```jsx
    <Dice />
    ```

- **Boilerplate Template**:
  - The script uses `cat > filename << EOF ... EOF` to write multi-line content to files.
  - **Master Component Template**:
    ```javascript
    import React from 'react';
    // Dynamic imports here

    const MasterComponent: React.FC = () => {
      return (
        <div className="MasterComponent">
          <h1>MasterComponent Component</h1>
          // Dynamic subcomponent rendering here
        </div>
      );
    };

    export default MasterComponent;
    ```

## Example

Let's create a master component named `Game` with subcomponents `Dice`, `Player_Cards`, and `Score_Board`.

1. **Run the Script**

   ```bash
   ./create_components.sh Game Dice Player_Cards Score_Board
   ```

2. **Script Output**

   ```
   Created master component directory 'Game'.
   Bootstrapped master component 'Game'.
   Created subcomponent directory 'Dice' inside 'Game'.
   Bootstrapped subcomponent 'Dice'.
   Created subcomponent directory 'Player_Cards' inside 'Game'.
   Bootstrapped subcomponent 'Player_Cards'.
   Created subcomponent directory 'Score_Board' inside 'Game'.
   Bootstrapped subcomponent 'Score_Board'.
   ```

3. **Resulting File Structure**

   ```
   Game/
     Game.tsx
     Game.css
     Dice/
       Dice.tsx
       Dice.css
     Player_Cards/
       Player_Cards.tsx
       Player_Cards.css
     Score_Board/
       Score_Board.tsx
       Score_Board.css
   ```

4. **Generated Code**

   - **Game/Game.tsx**

     ```javascript
     import React from 'react';
     import Dice from './Dice/Dice';
     import Player_Cards from './Player_Cards/Player_Cards';
     import Score_Board from './Score_Board/Score_Board';

     const Game: React.FC = () => {
       return (
         <div className="Game">
           <h1>Game Component</h1>
           <Dice />
           <Player_Cards />
           <Score_Board />
         </div>
       );
     };

     export default Game;
     ```

   - **Game/Dice/Dice.tsx**

     ```javascript
     import React from 'react';

     const Dice: React.FC = () => {
       return (
         <div className="Dice">
           <h2>Dice Component</h2>
         </div>
       );
     };

     export default Dice;
     ```

   - **Game/Player_Cards/Player_Cards.tsx**

     ```javascript
     import React from 'react';

     const Player_Cards: React.FC = () => {
       return (
         <div className="Player_Cards">
           <h2>Player_Cards Component</h2>
         </div>
       );
     };

     export default Player_Cards;
     ```

   - **Game/Score_Board/Score_Board.tsx**

     ```javascript
     import React from 'react';

     const Score_Board: React.FC = () => {
       return (
         <div className="Score_Board">
           <h2>Score_Board Component</h2>
         </div>
       );
     };

     export default Score_Board;
     ```

## Tips

- **Importing the Master Component**: You can now import the `Game` component into your application's main file (e.g., `App.tsx`) and render it.
  ```javascript
  import React from 'react';
  import Game from './Game/Game';

  const App: React.FC = () => {
    return (
      <div className="App">
        <Game />
      </div>
    );
  };

  export default App;
  ```
- **Customizing the Templates**: Feel free to modify the script to change the boilerplate code according to your project's standards.
- **Component Styling**: The generated `.css` files contain basic class selectors. You can expand them as needed.

