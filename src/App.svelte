<script>
    import { onMount } from "svelte";

    const items = ["아무 색깔", "흰색 얼음", "파란색 얼음", "차례 건너뛰기"];

    /** @type {HTMLCanvasElement} */
    let canvas;

    /** @type {HTMLElement} */
    let button;

    onMount(() => {
        const ctx = canvas.getContext(`2d`);

        const cw = canvas.width / 2;
        const ch = canvas.height / 2;
        const arc = Math.PI / (items.length / 2);

        for (let i = 0; i < items.length; i++) {
            ctx.beginPath();

            if (i % 2 != 0) {
                ctx.fillStyle = "#87CEEB";
            } else {
                ctx.fillStyle = "#D3D3D3";
            }

            ctx.moveTo(cw, ch);
            ctx.arc(cw, ch, cw, arc * (i - 1), arc * i);
            ctx.fill();
            ctx.closePath();
        }

        ctx.fillStyle = "#fff";
        ctx.font = "20px Pretendard";
        ctx.textAlign = "center";

        for (let i = 0; i < items.length; i++) {
            const angle = arc * i + arc / 2;

            ctx.save();

            ctx.translate(cw + Math.cos(angle) * (cw - 50), ch + Math.sin(angle) * (ch - 50));

            ctx.rotate(angle + Math.PI / 2);

            items[i].split(" ").forEach((text, j) => {
                ctx.fillText(text, 0, 30 * j);
            });

            ctx.restore();
        }
    });

    function rotate() {
        button.style.display = "none";

        const rotate = Math.floor(Math.random() * (360 * 2));

        canvas.style.transition = `2s`;
        canvas.style.transform = `rotate(-${360 * 3 + rotate}deg)`;

        setTimeout(() => {
            canvas.style.transition = `0s`;
            canvas.style.transform = `rotate(-${rotate}deg)`;
            button.style.display = "inline-block";
        }, 2500);
    }
</script>

<div class="roueltte">
    <canvas width="300" height="300" bind:this="{canvas}"></canvas>
</div>

<div>
    <button
        bind:this="{button}"
        on:click="{() => {
            rotate();
        }}">룰렛 돌리기</button>
</div>

<style>
    div {
        text-align: center;
        margin-top: 60px;
        margin-bottom: 60px;
    }

    .roueltte::before {
        content: "";
        position: absolute;
        width: 10px;
        height: 110px;
        border-radius: 5px;
        background: #000;
        top: -20px;
        left: 50%;
        transform: translateX(-50%);
        z-index: 22;
    }
</style>
