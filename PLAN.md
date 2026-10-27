# Scribble Hunt — plan

Top-down stickman hide-and-seek + Pictionary in a maze, for a group of friends in the browser.

## Teams
- **Drawers**: walk-to-draw. Hold Space (or left mouse) and your stickman leaves a paint trail behind its feet. Each drawing is one secret word.
- **Hunters**: just catch drawers by touching them. They share vision with each other. Optional bot hunters (host adds them in the lobby).

## Vision / fog of war
- Everyone sees a cone in front of them (100°, ~390px), plus a small circle around their feet. Walls block sight.
- Drawers see only their own cone. Hunters see the union of every hunter's cone.
- **Paint memory**: you only see paint you've personally spotted, both in the world and on your minimap. Unexplored parts of the minimap stay fogged. Your own paint is always known to you.

## Round flow
1. **Lobby**: the host shares a 5-letter room code, players pick teams, and the host sets round length, points to win and hunter head start.
2. **Play** (timer): hunters are frozen and blind during the head start. Drawers pick 1 of 3 words, paint, and **F banks** the drawing (or skips an empty word). Ink is limited per drawing; running out auto-banks.
   - **Mid-round guess**: type it in chat. Correct = **+2**. Your teammate's drawing is "cracked" for you, and the word isn't revealed to anyone else.
   - **Caught** = out for the round. Your *unbanked* drawing is erased. Banked drawings survive.
3. **Reveal** (Gartic-album style): each banked drawing is shown one at a time. Every drawer except its artist sees **only the parts they spotted** and types a guess (25s). Then the full drawing, the word and everyone's guesses are revealed. Correct = **+1** each. Caught drawers still guess here.
4. **Result**: drawers win if team points ≥ target (default: 3 × number of drawers). Otherwise hunters win.

## Controls
WASD/arrows move · mouse aims your cone (keyboard-only players auto-face movement) · hold right mouse = walk toward cursor ·
Space/left mouse = paint · F = bank drawing · C = wipe current drawing · Enter = chat/guess · Q = big map · Tab = switch spectate target (when caught)

## Tech
- Single `index.html`, no build step. Networking via **PeerJS** (WebRTC P2P, free public signaling server).
- **Host-authoritative**: the host's browser runs the game (catches, ink, words, scoring, reveal timing, bot AI) on a Web Worker timer so it isn't throttled.
- **Client-authoritative movement** (for zero-lag drawing; fine among friends). Clients send position, aim and pen at 30Hz; the host broadcasts state + ink ops at 30Hz.
- The maze is generated from a seed (recursive backtracker + extra loops + open "canvas rooms" + a central hunter den), so only the seed is sent.
- Fog/paint-memory is computed client-side. The client receives all ink and decides what it has "seen". Cheating is possible but it's a friends game.

## Ideas for later (suggestions, not commitments)
- Teams swap after each round; best-of-N match scoring.
- Hunter abilities (flashlight sprint, footprint trail).
- Drawer abilities (smoke bomb, decoy scribble).
- Maze size scaling with player count; map themes.
- Touch controls.
