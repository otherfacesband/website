+++
title = "Album"
+++

<div style="display: inline-block; perspective: 1500px;">
    <div id="album-cover-flip" style="position: relative; display: inline-block; transform-style: preserve-3d;">
        <img src="/albumcover.webp" style="display: block; backface-visibility: hidden;">
        <img src="/SleeveBack.webp" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; backface-visibility: hidden; transform: rotateY(180deg);">
    </div>
</div>
<div id="album-flip-text" style="cursor: pointer; opacity: 0.6; margin-top: 8px;">[flip]</div>
<!-- no blank lines inside this <script> block -- Hugo's --minify JS minifier fails with
     "unexpected < in expression" once a script here reaches a certain size AND contains a
     blank line (confirmed by bisection; not about any specific statement's content). Keep
     this script blank-line-free; group related lines with brief // comments instead. -->
<script>
(function () {
    var flipEl = document.getElementById('album-cover-flip');
    var flipText = document.getElementById('album-flip-text');
    if (!flipEl || !flipText) { return; }
    // preload the back image so it's already decoded by the time the first flip happens --
    // otherwise the browser fetches/decodes it on demand and it visibly pops in
    var preload = new Image();
    preload.src = '/SleeveBack.webp';
    // --- flip (deliberate, via the [flip] text only) ---
    var FLIP_MS = 900;
    var FLIP_EASING = 'cubic-bezier(0.68, 0, 0.27, 1.1)'; // an "overshoot and settle" curve
    var flipped = false;
    // --- hover/hold tilt (the "holo card" effect, tracks pointer position on the card) ---
    var MAX_TILT = 2;   // degrees of tilt at the card's edge; raise for a more dramatic effect
    var RESET_MS = 400;  // how long the card takes to settle flat again on release
    var tiltX = 0, tiltY = 0;
    var tracking = false;
    var flipping = false; // true for the duration of a flip -- locks out tilt input meanwhile
    function render(transitionCss) {
        flipEl.style.transition = transitionCss;
        var flipAngle = flipped ? 180 : 0;
        flipEl.style.transform = 'rotateX(' + tiltX + 'deg) rotateY(' + (flipAngle + tiltY) + 'deg)';
    }
    function updateTilt(clientX, clientY) {
        if (flipping) { return; } // don't let a mouse/touch move stomp on the flip transition
        var rect = flipEl.getBoundingClientRect();
        var nx = (clientX - rect.left) / rect.width - 0.5;  // -0.5 .. 0.5 across the card
        var ny = (clientY - rect.top) / rect.height - 0.5;
        tiltY = nx * MAX_TILT;
        tiltX = -ny * MAX_TILT;
        render('none'); // no transition while actively tracking -- it should feel instant
    }
    function resetTilt() {
        tracking = false;
        if (flipping) { return; } // same -- let the flip's own transition finish untouched
        tiltX = 0;
        tiltY = 0;
        render('transform ' + RESET_MS + 'ms ease-out');
    }
    function toggle() {
        if (flipping) { return; } // ignore extra clicks until the current flip finishes
        flipping = true;
        flipped = !flipped;
        tiltX = 0;
        tiltY = 0;
        render('transform ' + FLIP_MS + 'ms ' + FLIP_EASING);
        setTimeout(function () { flipping = false; }, FLIP_MS);
    }
    // mouse: tilt follows the cursor while hovering
    flipEl.addEventListener('mousemove', function (e) {
        tracking = true;
        updateTilt(e.clientX, e.clientY);
    });
    flipEl.addEventListener('mouseleave', resetTilt);
    // touch: tilt follows your finger while held down on the card
    flipEl.addEventListener('touchstart', function (e) {
        tracking = true;
        updateTilt(e.touches[0].clientX, e.touches[0].clientY);
    }, { passive: true });
    flipEl.addEventListener('touchmove', function (e) {
        if (!tracking) { return; }
        updateTilt(e.touches[0].clientX, e.touches[0].clientY);
    }, { passive: true });
    flipEl.addEventListener('touchend', resetTilt);
    flipText.addEventListener('click', toggle);
})();
</script>
<br> <br>

### <b>our first album </b>
On their self-titled debut LP, Other Faces took to the studio to reimagine a body of work built from close improvisation, experimentation, and live performance. Recorded at Honey Jar studio in Brooklyn, NY, the six-track album balances nature with analog grit, moving seamlessly between dust-lit ambient interludes to punchy percussion-driven madness. With <i>Other Faces</i>, the trio continues their push to redefine the barriers between sound and visuals.

<p style="text-align: right;">
<b>other faces will be available November 2026 <br>
listen on <a href="https://otherfacesband.bandcamp.com/album/other-faces"><b><u>bandcamp</u></b></a> or your favorite streaming service. </b>
</p>
<br><br>

### <b>Track List </b>
Intro <br>
Field <br>
Friction <br>
Distance <br>
Hold <br>
Contact <br>

### Credits <br>
<b>Compositions</b> <br> Dan Langa (1-3, 6), Adam Lutz (4), and Krisitian de Leon (5)

<b>Performances</b> <br> Dan Langa, Adam Lutz, and Kristian de Leon <br>
Riley Palmer: Crotales, Marimba, Vibraphone (2, 4) <br>
Zachary Mezzo: Violin (3, 5) <br>
Matt Evans: Drumset (6) <br> 

Produced by Adam Lutz, Dan Langa, and Kristian de Leon <br>
Mixed by Dan Langa (1-3, 5-6), Adam Lutz (4) <br>
Additional Mixing by Kristian de Leon <br>
Mastering by Taylor Deupree <br>

Album art by Kristian de Leon

Special Thanks to Sō Percussion, Tim Thomas; Souren and Toby; cmntx records. 

<i>In memory of Tim Thomas – we are forever grateful for the support and encouragement for getting this project off the ground. </i>
