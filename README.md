# Battle Node

Battle Node is a simple node.js library built for Battle Eye Rcon.

Battle Eye is an anti-cheat that runs on popular game-servers such as ARMA2, ARMA3 and Dayz Mod.

This library allows you to send and receive aynschronous commands to your game-server.

[List of Commands](http://www.battleye.com/doc.html)

#### How to Use

```bash
git clone https://github.com/Lulz0rz/battle-node.git
cd battle-node
node example.js // runs example
```

```javascript
var BattleNode = require('./lib');
var fs = require('fs');

var config = {
  ip: '127.0.0.1',
  port: 2302,
  rconPassword: 'testing'
};

var bnode = new BattleNode(config);

bnode.login();

bnode.on('login', function(err, success) {
  
  if (err) { console.log('Unable to connect to server.'); }

  if (success == true) {
    console.log('Logged in RCON successfully.');
  }
  else if (success == false) {
    console.log('RCON login failed! (password may be incorrect)');
  }
            
});

bnode.on('message', function(message) {
  
  console.log(message);
  
});

// send commands once connected
setTimeout(function() {

  bnode.sendCommand('version', function(version) {
    console.log('Battle Eye Version ' + version);
  });
  
  bnode.sendCommand('bans', function(bans) {
    
    fs.writeFile('bans.txt', bans, function (err) {
      if (err) console.log(err);
      
      console.log('Saved bans to bans.txt');
    });
                            
  });
  
  bnode.sendCommand('players', function(players) {
    console.log(players);
  });

  
  bnode.sendCommand('say -1 Hello World');
  
}, 1000);

bnode.on('disconnected', function() {
  
  console.log('RCON server disconnected.');
  
});
```
