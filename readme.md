# Base Cube

Draft - plain markdown without Images or Links 
---

My name is Valerie. I have a goal to become a full stack creative technologist. I'm currently building a portfolio of projects aligned with data/music visualizations, art experiments, and onchain apps outside of the lens of financialization. I'm using this bootcamp as a way to deepen my learning of Solidity, Base network, and connect with those that can help build that vision.

I built Base Cube, an onchain app that visualizes data from the Base Network and allows people to playfully interact with Base through a Rubik’s cube game.

It does this by:
- Animating a Rubik’s cube solving sequence based on new blocks
- Allowing free play with the cube
- Having a competitive timed mode

## _exploration_

I got the idea to do this project because I wanted to do something that was stimulating and straightforward, with a lot of pre-existing open source examples that I could remix as a commemoration to Base.

I started thinking about different objects that I could potentially animate using data from Base and got the idea of the Rubik's cube while watching all of the motion graphics in the videos throughout the camp and studying the different permutations of the token art that you get after completing each exercise.

To figure out how I wanted Base Cube to look like I went on an exploration researching and looking at different inspiration images of creative cubes that resembled a direct remix of the token art, kind of using that conical-monochromatic gradient that could lay on the different planes of the cube in a way that was tasteful and pretty.

_shows example inspiration images_

## _creating the first draft of the cube_

Once I had this vision board from the exploration, the process for making the first draft of the cube was a mix of learning/using Spline3d (for the textures), Midjourney (for varied and specific references) and fine-tuning the different generations of the images with my Photoshop skills. 
I also used the branding kit within their GitHub that helped me get the exact color I needed to make different gradients for the cube.

_clips of me trying to blend the images together and then put it into a gaussian splatter to make it a 3d-like image for 3D AI_

## _creating a custom shader_

WIth the first draft of the cube, I needed to make a shader/texture for the actual 3D asset that would need to go on the site. I struggled with deciding if i wanted to use Blender, ThreeJS, or Spline to create the cube. If I used Blender I feared I would not be able to easily integrate blockchain events into the animation sequence, like I would need a creative environment that worked well with Javascript/Typescript since it's what I was most familiar with in my tech stack and Blender worked better with Python. 

For the amount of code examples of a interactive cube I could find they all used ThreeJS and to take the drafted image and be able to recreate it with 3JS that meant understanding the traditional map of colors for a Rubik's cube and modifying it to show the angled gradient. Because I was on a time constraint I did not feel like doing all of the colors for each side of the cube by hand and chose a path with Spline that takes one shader/texture wrapped over a 3D object. 

## [[_creating a minimal animation sequence in Spline 3D_]]

Creating a full visualization using a stream of data on Base would require more development time. For this initial proof of concept, I want to demonstrate the basic capability of reading events from a blockchain and triggering actions based on that data.

For the MVP, I set up a 12 second predetermined animation loop showing a Rubik's cube going from a scrambled state to solved state. The sequence of moves consists of...

*Ex: F, R, U, L, B, D, F',R -> Solved, then it scrambles again and starts from the top.* 

The integration with blockchain: For every 3 new blocks read from Base, the animation will advance 1 step forward in the preset sequence. And that's it so far. 

Some UI inspirations to make it clearer that there is blockchain present: A ticker, similar to the ticker on the ZORA API site. *But I will come back to this animation when talking about the build for the landing page*

## _creating the contract for the timed game_

The core smart contract:

- Is a ERC721 that represents a player's identity and stats in the timed play game
- Stores relevant metadata on-chain, such as:
    - Player's Ethereum address / ENS name
    - Number of games played
    - Best time solving a cube
- Has a minting function that allows a player to mint a new token, starting their stats tracking
- Has a function that invalidates the continued use of NFT to allow a player to add to a leader board if the player has to reset their rubix cube game

*see leaderboard  *

## _creating a frontend and mint experience for Base Cube_

I started researching different ways that people have created apps for both interactive 3D objects and Rubik's cubes in the past. The inspiration for the landing page comes from Shuding, an engineer at Vercel who creates creative libraries and components.

For the landing page of the app, I wanted to create an animated sequence where a scrambled Rubik's cube gets solved by reading live data from the blockchain. In this example, parts of the cube's motion sequence are allowed to continue from verifying if a set amount of latest block numbers on Base has been created.

There are also inspirations from open source Rubik's cube projects and different data visualization products (EVMBEAT, Blocks mint, Zora API Funnel TX Street (blockchain data visualized via South Park character) and a Zorb Visualizer created by me for the Zora API hackathon using NFT image data.)

### _AssemblyPress_ - onchain apps framework 

I wanted to use AssemblyPress to build the Rubik's Cube application. AssemblyPress is a framework that integrates smart contracts as a backend database, enabling people to build frontend apps to get blockchain data very fast.

Being able to retrieve speed is important for data visualizations and onchain games, even metaphorically the culture around rubiks cube competitions is solving the cube as fast as possible, we want our onchain apps to run in the same way. 

For most full stack NFT tutorials as of right now they instruct you to ping RPC nodes to get data directly from the chain or through an indexer which can be expensive and slow but with the AssemblyPress framework instead of doing access control checks onchain, it pings a database to check permissions and requirements first, then starts a process similar to more classic web 2 techniques of talking to a database, and once you have all of your data in one place you can reuse the data for multiple applications by create a simple API to pull in things to your App. This structure is a lot faster and more performant. 

### _interactive play_

I was inspired by BasePaint - an interactive web3 art platform where people mint paintbrushes to collaborate on canvases and earn money. I wanted to incorporate a similar free access and paid access route where you can explore but if you actually want to save progress to the leader board u need to mint a game card. When you enter the app it pulls up a cube that you can interact with with your mouse

so for the free play mode theres no need to connect your wallet you can just enjoy tinkering with the cube. but if you want to do timed you would have to go to the drop down and connect your wallet to mint a timed game 

Going through the leaderboard 

- Retrieves the metadata for each player's NFT
- Allows players to pseudonymously compete, since it references the onchain data rather than asking for names/ids
- Ranks players by fastest puzzle solve
- Updates as players mint new NFTs and old ones become invalidated.
