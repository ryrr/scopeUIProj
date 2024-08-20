# scopeUIProj
UI project for scope labs
# Overview
- Decided on a temporary name of "Ballpark" for the app.
- Allows for the visualization of attended Baseball games and the user-uploaded media associated with each game.
- Provides guidelines for the implementation of a filtering system on the games users have logged.
- Users are able to see high level statistics at a glance in the "Stats" section and the games they have attended (along with some metadata) in the "Games" section.
# Design Decisions & Responsiveness
- I elected to design this layout to fit a large tablet, I chose this because I think it shows a good middle ground between my vision for both the Desktop and Mobile experiences
- The instructions stated to pick Mobile OR Desktop and I realize that a tablet design may be seen as not adhering to the specs. However I would argue that I really designed for mobile first, and chose the tablet only to more easily show the resposive transition to a Desktop view.
- I broke the app down into two main sections, Games and Stats as per the Specs
## Games section
- One liberty I took while designing is not having an initial media preview for each game displayed. Some reasons for this include initial page load times and increasing the density of information on the page. If I had elected to include a carousel or flip book style preview on each un-expanded game entry, it would require a lot more scrolling. However for Desktop where screen space is plentiful, I think I would make the game rows bigger and perhaps include either a carousel or preview image.
- Related to the previous point is how I DID choose to to the media display. I chose to display the game information in a classic spreadsheet form, where each row can be clicked and expanded to show a dense grid of images associated with that game. One reason I chose to do it this way is because in my view, uploaded images may come in different sizes/orientations and displaying them in slideshow or carousel format would look kinda bad. The grid I've described in the design is a classic way of displaying image sets like this and can be seen on popular image hosting sites like Pinterest and VSCO.
- I described some functionality related to the filtering of the games section, showing that the user should be able to view games against specific teams and games that fall in a date range. I thought about adding it to the stats section as well to view stats over a specific interval, however the specs specified that the stats should be high level and historical so I left it out (however it would be easy to add).
- I also chose this layout because it's tried, true and scales up and down very well, albeit with some columns potentially having to be cut off on smaller screen sizes. However we don't have too many columns so it shouldn't be much of an issue. It also fits the flow I've imagined in my head where a user wants to find a specific game they went to by filtering a date range and perhaps toggling the acc/dec feature on a column to find a specific high scoring game they want the pictures for. This layout shows a lot of information compactly and scales well.  
## Stats section
- I kept the stats section relatively basic, consisting of a couple cards in a scrollable section containing some high level information about the games a user has attended
- I also took a risk on the font choice used for the numbers displayed in this section. While I was working on the design I kept thinking about how I could make it more fun, since it's an app about a game after all! To warm up and game-ify the design a little bit I went with this funky font that reminded me of human handwriting. To me this evokes the feeling that I or the user actually filled this information in themselves as they went to each game, erasing the previous entry and scrawling in an update as something notable happens. I drew inspiration from a golf card that you pencil in as you go. The sloppiness and silliness of it helps to translate the experience to filling out a journal while you're at each game, which to me makes it more fun! I was trying to imagine what a yearly Yankees scrapbook might look like if translated to the web. I have a couple ideas about taking the design even further in this direction expounded on in the next section.
- I also imagine that the stats section would be larger on desktop, showing most if not all of the available stat cards in perhaps multiple rows if necessary
## Errata 
- It also may be unclear and half-baked but I made the Yankees logo in the Header clickable, which would in turn let you change the team affiliation and app theme to whichever MLB team you wanted.
- the stats section is collapsable in case a user just wants to look at the games section
- hamburger menu in the top left is just there with no thought out functionality per specs only describing the home screen
# Expansions & Flourishes 
## Stats expansion 
- I find the stats in this design to be perhaps a little too simplistic but I kept it this way so as to not delve too deep into making charts and graphs look good in Figma.
- It would be really cool to have some long term graphs and charts related to game attendance, weather, deeper baseball stats and many other things I could think of but left out.
- I would also love to make the stats section feel more journaled/handmade with stickers and collectables ex. Make the fav player headshot look like a sticker you collected and pasted in a journal
- I would also like to add functionality involving the "pinning" of stats you like to look at the most so they're always on your screen
- I also had the idea that you could add an animation that looked like an eraser on a dry erase board when a stat was updated after you logged a game. Imagine the number being erased and penciled in again over it. Would be cool!
- Also thought about adding more advanced filtering to stats but ignored it for the sake of time.
## Recents section and Sharing link
- Didn't touch on this in the design because the specs said to stick to Stats and Games but I think it would be cool to have a display for the recent images you've uploaded as well as have some interface for creating a shareable image or link for social media.
- I imagine the recents section as the grid seen in the games section but horizontal rather than vertical and perhaps an animated scroll as the images drift by kinda like the Apple TV screensaver.
- The shareable feature would let you pick a game and some stat cards and combine them into an image that includes media from the game and the stat cards you've selected.
## Ballpark selector and theming 
- I made the Yankees logo in the header clickable to (in theory) change your team preference as well as the theme of the app. Each team has primary secondary and tertiary colors which makes it easy to theme dynamically.
- I used Yankees colors for the specific example but other team colors could be used and applied easily with some variables
## Games expansion
- Could perhaps add more fields and a different row display format on desktop, given that we have more space to work with ie: show preview images at a glance
- Also could add favoriting functionality to specific games  
# Translation into React
- In here I'll outline my approach to creating each section in React, list components and shape and maybe a high level overview of how some systems might work / be implemented
## High level layout and comps
- Three main areas `header` `stats` and `games`
- would probably lay these out in a flex column container top to bottom
- component list
  - `home`: our brain component for the home screen, responsible for layout and querying the data from the BE
  - `stats`: responsible for stats section layout and mapping in the stat cards (could potentially do w/o because stats is pretty simple)
  - `stat-card`: component that displays a stat, takes in a title and text or `stat-display` component to render 
  - `{name}-stat-display`: component that goes inside stat card to display a graph or something more complex than text
  - `games-section`: responsible for games layout and setting games filters as well as mapping in the game rows
  - `game-row`: Individual game row component that gets mapped in, also contains `photo-display` instance
  - `photo-display`: Scrollable photo grid to display media, could potentially be re-used elsewhere hence the separation from `game-row`
## Stats
- Would also probably be a flex container on mobile views, however using a grid could be a good choice if we want to arrange the cards in rows and columns on desktop
- Stats flows the whole view width when scrolled, with cards overflowing or disappearing off screen. Design can be accomplished easily with the overflow property, margin adjustment and fixed display
- Each card would be an instance of a `card` component which are given props by a parent `stats-section` (or `home`) that describe their title and contents (the contents may be another component entirely, see `stat-display`)
- `home` would be our root component responsible for querying and passing game and stats data to the sections
- I imagine stats would be retrieved as formatted JSON from the backend upon request (on page load or upload)
## Games 
- Would use a CSS grid to display the rows and columns that represent Games, although sometimes it can be tricky to get spacing and alignment totally right with text in rows like this. One solution would be to limit fields to a certain length on mobile
- Also imagine games are retrieved from the backend as JSON and mapped into respective `game-row` components
- Again this info would be sent by `home` to `games-section` which would map in the `game-row`s
- On click the `game-card` will expand and show the `photos-grid` component which is built with css grid, ensuring that photos are displayed in the orientation that they are meant to. I've bult a small demo app to show that this is possible
- `photo-display` should be of a fixed height and be scrollable as to not take up a ton of the screen and perhaps allow images to load as you scroll
## Modals
- On photo click a `photo-modal` should appear with the full size image, should include controls for moving to the next or prev image if available
- Might also have a modal for filtering depending on complexity, if simple no modal is really needed just a team and date picker
- Can be accomplished by setting a current game variable to the game that album is from and then looking at currentGame[images[imdIdx]] and inc/dec imgIdx to select the appropriate image for display
## Filtering 
- Teams filter and date ranges can be added to Games and can be removed by clicking the X on the filter bubble
- can be accomplished trivially using a filteredGames useState that processes the larger games object when a filter is applied or removed 
# Pitfalls and Grey Areas
- I've used Figma before but I am by no means an expert, may not have adhered to all the best practices, certainly tried to use auto-layout as much as I could
- Responsive design always has some pitfalls like text getting too close in a row, etc... didn't delve too deeply into this but hopefully demonstrated how my design scales
- Since the design is mobile first I would image we don't use the entire screen width on large desktops ie: big side margins, I think this is ok
- Since I didn't actually translate this into React there may be some things I'm missing
- Many ideas went unimplemented due to time constraints but hopefully my vision for expansions and features is clear
- I used 4 different fonts which is kind of a lot. logo/header: Rhodium Libre / Headers: Habibi / Numbers and Scores: Single Day / General or Default: Lexend Deca. However I think they work together fairly harmoniously. Did it this way because it made it more fun
- Lack of a photo preview on each game row might be perceived as a problem but I made a choice and outlined my reasoning above.
# Wrap up
- Overall had a lot of fun making this and definitely would've liked to actually code it out and create the flourishes I mentioned
- Hope my explanations and liberties are reasonable and that my Figma was up to par with what you guys are looking for.
- Hope you enjoyed it!
