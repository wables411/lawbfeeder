// Initialize Phaser game configuration
const config = {
    type: Phaser.AUTO,
    width: 800,
    height: 600,
    scene: {
        preload: preload,
        create: create,
        update: update
    },
    physics: {
        default: 'arcade',
        arcade: {
            gravity: { y: 200 },
            debug: false
        }
    }
};

// Create game instance
const game = new Phaser.Game(config);

// Global variables
let player;
let emojis;
let cursors;
let score = 0;
let scoreText;
let timeLeft = 120; // 2 minutes in seconds
let timeText;
let gameOver = false;
let music;
let musicButton;
let lawbImage;
let restartButton;

function preload() {
    this.load.image('background', 'path/to/background.png');
    this.load.image('lawb', 'images/lawb.png');
    this.load.image('emoji', 'path/to/emoji.png');
    this.load.audio('gameMusic', 'path/to/game_music.mp3');
    this.load.image('musicPlay', 'path/to/play_emoji.png');
    this.load.image('musicPause', 'path/to/pause_emoji.png');
}

function create() {
    // Add background
    this.add.image(400, 300, 'background');

    // Add player (lawbster)
    player = this.physics.add.image(400, 550, 'lawb');
    player.setCollideWorldBounds(true);

    // Add emojis group
    emojis = this.physics.add.group();

    // Add score text
    scoreText = this.add.text(16, 16, 'Score: 0', { fontSize: '32px', fill: '#000' });

    // Add timer text
    timeText = this.add.text(16, 56, 'Time: 2:00', { fontSize: '32px', fill: '#000' });

    // Set up cursor keys
    cursors = this.input.keyboard.createCursorKeys();

    // Set up emoji spawning
    this.time.addEvent({
        delay: 1000,
        callback: spawnEmoji,
        callbackScope: this,
        loop: true
    });

    // Set up game timer
    this.time.addEvent({
        delay: 1000,
        callback: updateTimer,
        callbackScope: this,
        loop: true
    });

    // Add music
    music = this.sound.add('gameMusic');
    music.play({ loop: true });

    // Add music control button
    musicButton = this.add.image(750, 570, 'musicPause').setInteractive();
    musicButton.on('pointerdown', toggleMusic);

    // Add lawb.xyz link
    lawbImage = this.add.image(50, 570, 'lawb').setInteractive();
    lawbImage.on('pointerdown', () => {
        window.open('https://lawb.xyz', '_blank');
    });
}

function update() {
    if (gameOver) {
        return;
    }

    // Player movement
    if (cursors.left.isDown) {
        player.setVelocityX(-160);
    } else if (cursors.right.isDown) {
        player.setVelocityX(160);
    } else {
        player.setVelocityX(0);
    }

    // Check for collisions between player and emojis
    this.physics.add.overlap(player, emojis, collectEmoji, null, this);
}

function spawnEmoji() {
    if (gameOver) {
        return;
    }

    const x = Phaser.Math.Between(0, 800);
    const emoji = emojis.create(x, 0, 'emoji');
    emoji.setBounce(0.7);
    emoji.setCollideWorldBounds(true);
    emoji.setVelocity(Phaser.Math.Between(-100, 100), 20);
}

function collectEmoji(player, emoji) {
    emoji.disableBody(true, true);
    score += 1;
    scoreText.setText('Score: ' + score);
}

function updateTimer() {
    if (gameOver) {
        return;
    }

    timeLeft -= 1;
    const minutes = Math.floor(timeLeft / 60);
    const seconds = timeLeft % 60;
    timeText.setText(`Time: ${minutes}:${seconds.toString().padStart(2, '0')}`);

    if (timeLeft <= 0) {
        endGame();
    }
}

function endGame() {
    gameOver = true;
    this.physics.pause();

    const gameOverText = this.add.text(400, 300, `Congratulations!\nYou fed your lawb ${score} emoji${score !== 1 ? 's' : ''}!`, {
        fontSize: '32px',
        fill: '#000',
        align: 'center'
    }).setOrigin(0.5);

    restartButton = this.add.text(400, 400, 'Restart Game', {
        fontSize: '24px',
        fill: '#000',
        backgroundColor: '#fff',
        padding: { x: 10, y: 5 }
    }).setOrigin(0.5).setInteractive();

    restartButton.on('pointerdown', restartGame, this);
}

function restartGame() {
    score = 0;
    timeLeft = 120;
    gameOver = false;
    this.scene.restart();
}

function toggleMusic() {
    if (music.isPlaying) {
        music.pause();
        musicButton.setTexture('musicPlay');
    } else {
        music.resume();
        musicButton.setTexture('musicPause');
    }
}
