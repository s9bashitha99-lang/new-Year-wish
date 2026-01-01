# new-Year-wish
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Happy New Year 2026</title>
<style>
    body {
        margin: 0;
        overflow: hidden;
        background: black;
        font-family: 'Arial', sans-serif;
    }

    canvas {
        display: block;
    }

    /* Glowing main message */
    .message {
        position: absolute;
        top: 30%;
        left: 50%;
        transform: translate(-50%, -50%);
        text-align: center;
        color: gold;
        font-size: 10vw; /* responsive font */
        font-weight: bold;
        text-shadow: 
            0 0 10px #ff0,
            0 0 20px #ff8,
            0 0 30px #f00,
            0 0 40px #f80;
        animation: glow 1.5s infinite alternate;
        pointer-events: none;
        white-space: nowrap;
    }

    /* Your wish message */
    .wish {
        position: absolute;
        top: 55%;
        left: 50%;
        transform: translate(-50%, -50%);
        text-align: center;
        color: white;
        font-size: 4vw; /* responsive font */
        line-height: 1.5;
        max-width: 90%;
        text-shadow: 0 0 5px #fff;
    }

    @keyframes glow {
        0% { text-shadow: 0 0 10px #ff0, 0 0 20px #ff8, 0 0 30px #f00, 0 0 40px #f80; }
        50% { text-shadow: 0 0 20px #ff0, 0 0 40px #ff8, 0 0 60px #f00, 0 0 80px #f80; }
        100% { text-shadow: 0 0 10px #ff0, 0 0 20px #ff8, 0 0 30px #f00, 0 0 40px #f80; }
    }
</style>
</head>
<body>

<canvas id="canvas"></canvas>

<div class="message">🎆 Happy New Year 2026 🎆</div>

<div class="wish">
    Happy New Year 2026! 🎉<br>
    Wishing you a year filled with peace, good health, and happiness from the heart ✨<br>
    May 2026 bring new beginnings, beautiful moments, and success in everything you do.<br><br>
    Let’s make this year a good one!<br>
    Your Faithfully,<br>
    Shehan Bashitha
</div>

<script>
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

window.addEventListener('resize', () => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
});

class Particle {
    constructor(x, y, color) {
        this.x = x;
        this.y = y;
        this.color = color;
        this.angle = Math.random() * 2 * Math.PI;
        this.speed = Math.random() * 4 + 1.5; // slower on mobile
        this.gravity = 0.05;
        this.life = 70;
    }
    update() {
        this.x += Math.cos(this.angle) * this.speed;
        this.y += Math.sin(this.angle) * this.speed + this.gravity * this.speed;
        this.speed *= 0.95;
        this.life--;
    }
    draw() {
        ctx.fillStyle = this.color;
        ctx.beginPath();
        ctx.arc(this.x, this.y, 2, 0, Math.PI * 2);
        ctx.fill();
    }
}

class Firework {
    constructor(x, y) {
        this.x = x || Math.random() * canvas.width;
        this.y = canvas.height;
        this.targetY = y || Math.random() * canvas.height / 2;
        this.color = `hsl(${Math.random() * 360}, 100%, 60%)`;
        this.speed = Math.random() * 2 + 3;
        this.exploded = false;
        this.particles = [];
    }
    update() {
        if (!this.exploded) {
            this.y -= this.speed;
            if (this.y <= this.targetY) this.explode();
        } else {
            this.particles.forEach(p => p.update());
        }
    }
    draw() {
        if (!this.exploded) {
            ctx.fillStyle = this.color;
            ctx.beginPath();
            ctx.arc(this.x, this.y, 3, 0, Math.PI * 2);
            ctx.fill();
        } else {
            this.particles.forEach(p => p.draw());
        }
    }
    explode() {
        this.exploded = true;
        for (let i = 0; i < 50; i++) { // fewer particles for mobile
            this.particles.push(new Particle(this.x, this.y, this.color));
        }
    }
}

let fireworks = [];

// Add fireworks on touch/click
canvas.addEventListener('click', e => {
    fireworks.push(new Firework(e.clientX, e.clientY));
});
canvas.addEventListener('touchstart', e => {
    e.preventDefault();
    const touch = e.touches[0];
    fireworks.push(new Firework(touch.clientX, touch.clientY));
}, { passive: false });

function animate() {
    ctx.fillStyle = "rgba(0,0,0,0.25)";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    if (Math.random() < 0.05) fireworks.push(new Firework());

    fireworks.forEach((fw, i) => {
        fw.update();
        fw.draw();
        if (fw.exploded && fw.particles.every(p => p.life <= 0)) {
            fireworks.splice(i, 1);
        }
    });

    requestAnimationFrame(animate);
}

animate();
</script>

</body>
</html>
