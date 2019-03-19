# Infinite Runner Iteration 1

## Week 1
For the first week  i mainly spent researching how to make a basic auto runner while using 
parts from [emoji-run](https://github.com/JAX3/emoji-run) to get a basic understanding on how to make an endless runner.


The main part of the week was also spent learning how to make the platform spawn before the player reaches them as I had some issue of the player moving to fast for the platforms to be generated though once this issue was fixed i had one major issue of the platform still existing within the world  and was causing some performance issues after 30seconds. I fixed this by writing a small method to reuse the platforms once they we're outof view of the player and camera.

```javascript
// reusing platforms out of view.
        let minDistance = game.config.width;
        this.platformGroup.getChildren().forEach(function (platform) {
            let platformDistance = game.config.width - platform.x - platform.displayWidth / 2;
            minDistance = Math.min(minDistance, platformDistance);
            if (platform.x < - platform.displayWidth / 2) {
                this.platformGroup.killAndHide(platform);
                this.platformGroup.remove(platform);
            }
}, this);

````
this allowed me to reused the platforms once they go outside the camera view. For the rest of the week was mainly spent working the player controls interms of player speed,jumping and gravity.
```javascript
 // global game options
let gameOptions = {
    platformStartSpeed: 325,
    spawnRange: [100, 350],
    platformSizeRange: [75, 250],
    playerGravity: 900,
    jumpForce: 400,
    playerStartPosition: 200,
    jumps: 3
}
```
having a global  game options allowed me to change certian option on the fly without editing each method such as the jumps being capped at ``3``.
```javascript
jump() {
        if (this.player.body.touching.down || (this.playerJumps > 0 && this.playerJumps < gameOptions.jumps)) {
            if (this.player.body.touching.down) {
                this.playerJumps = 0;
            }
            this.player.setVelocityY(gameOptions.jumpForce * -1);
            this.playerJumps++;
        }
}
```
## Week 2
For week 2 i mainly spent my time working on the smaller thing such as the player artwork and a timer system ![player sprite](https://github.com/JAX3/Runner-game-Iteration1/blob/master/player.png) 
```js
var text;
var timedEvent;
//timer
 text = this.add.text(32, 32);
 timedEvent = this.time.addEvent({ delay: 10000,  callback: counter,  repeat: 10, startAt: 8000 });
  text.setText('Time: ' + timedEvent.repeatCount(+1).toString().substr(0, 4));
  function counter(event) {
    //text.setText('Event.progress: ' + timedEvent.getProgress().toString().substr(0, 4);
    console.log('Event.progress: ' + (timedEvent.repeatCount));
}
```
this was my basic way of timing the game  however the phaser 3 ``timedEvent`` function has a property called ``elapsed`` which prints out the number of seconds that has passed. However it wouldn't allow me to access that property and would print out ``nan``  or ``undefined`` so for [Interation 2](https://github.com/JAX3/interation2)  I will create a better and easier to follow timing system.
