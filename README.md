# Buzzer! 🔔

A classroom quiz buzzer for the teacher's computer and the students' iPads / Chromebooks / phones. Teams join with a QR code, pick a unique mascot and a unique sound, then race to buzz in. The teacher's screen plays only the sound of whoever buzzed **first**, shows the full buzz order (1st to last) and keeps a bright, animated scoreboard.

It is one static file (`index.html`), no server of your own needed. Same hosting style as ClassBoard.

## Deploy on GitHub Pages

1. Create a new GitHub repository (e.g. `buzzer`).
2. Upload `index.html` (and this README) to the repo root.
3. **Settings → Pages →** *Deploy from a branch* → `main` / `(root)` → Save.
4. After a minute it is live at `https://<your-username>.github.io/buzzer/`.

GitHub Pages is HTTPS, which the **record your own noise** feature needs (browsers only allow the microphone on secure pages).

## Running a game

**Teacher**
1. Open the site on the computer that is connected to your speakers and press **Open hub**.
2. Show the QR code (or the 6-letter room code). Teams appear as colourful chips as they finish picking. Click a chip to hear that team's sound.
3. Press **Start game**. The buzzers start **LOCKED**.
4. Flip the big switch (or press the **Space bar**) to go **LIVE!**. Each flip to live starts a fresh round and clears the buzz list.
5. The first team to buzz triggers a full-screen celebration and **only their sound plays**. The **Buzz order** panel lists 1st → last with the time gap behind the winner. Tap 🔊 on any row to replay a team's sound.
6. Pick how many points a tap is worth (**1 / 2 / 3 / 5 / 10**), then press **+** on a scoreboard card or on a row in the buzz order. **−** takes points off.
7. Flip the switch back to **LOCKED** to stop buzzing. The list stays on screen so you can award points. **Clear** empties it without changing the lock.

Other controls: **Rank** turns automatic leaderboard sorting on or off, 🔊 mutes the hub, **QR** re-shows the join code for latecomers, **Reset** sets every score to 0, **End** closes the room, and the ✕ on a card (appears on hover) removes a team.

**Students**
1. Scan the QR code (or open the site, type the room code, press **Join**).
2. Type a name or team name → pick a **mascot** (30 to choose from) → pick a **sound** (30 built in, tap to hear each) or tap **Record your own noise** (3 seconds).
3. A mascot or sound that another team already holds is greyed out with a **TAKEN** badge, live. Recorded noises are always unique, so any number of teams can record.
4. Press the giant buzzer when it is live. It is grey and shakes while locked. After buzzing, the student sees their place (1st, 2nd, 3rd…).

A student who closes the tab and comes back with the same link gets straight back in with the same team and score. The teacher can also refresh the page without losing the room, the teams or the scores (same room code).

## The 30 sounds

Boing, Laser, Coin, Bell, Honk, Quack, Meow, Drum, Slide Whistle, Rocket, Zap, Fanfare, Doorbell, Cuckoo, Bubble Pop, Wobble, Ding, Robot, UFO, Splat, Crash, Tweet, Siren, Xylophone, Power Up, Sad Trombone, Whoosh, Dolphin, Whoopee, Sparkle.

They are generated live in the browser with the Web Audio API, so there are no audio files to host, and nothing to download.

## How it works

Students connect to the teacher directly over WebRTC using [PeerJS](https://peerjs.com). The free public PeerJS server is only used for the first handshake; buzzes then go device-to-device. The teacher's browser is the referee: it decides the order in which buzzes *arrive* and ignores a second buzz from the same team in a round.

## Notes and limits

- **Fairness:** the order is set by when each buzz reaches the teacher's computer. On a normal school Wi-Fi the difference between devices is a few milliseconds to a few tens of milliseconds, so it is very fair for a classroom game, but it is not a TV-studio timing system. Keep every device on good Wi-Fi.
- **Sound on the teacher's computer:** browsers only play sound after the page has been clicked once. Pressing **Open hub** does that, so you are covered. If you refresh the teacher page, click anywhere once before the first round (the game start button does it).
- **iPad silent switch:** if students hear nothing when previewing sounds, the side switch may be on silent.
- **Recorded noises** are shared as small audio clips (about 3 seconds). They live only in the teacher's page memory and are not stored anywhere.
- **School networks:** some block peer-to-peer traffic. If students can't connect, add a TURN server in `PEER_OPTS` near the top of the script, or self-host a PeerServer (both are commented there).
- **Reliability of the public PeerJS server:** it is free and best-effort. For heavy daily use, self-host `peerjs-server`.
- Anyone with the room code can join, so only show the QR to your class. ✕ on a card removes a team and **End** closes everything.

## Customising

Everything is in `index.html`. The 30 mascots (emoji, name and two team colours) are in the `MASCOTS` list, and the sounds are in `SOUNDS`. Each sound is a few lines of oscillator/noise code, so it is easy to tweak or swap one.
