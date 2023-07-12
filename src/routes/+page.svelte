<script lang="ts">
    import { Canvas, Layer, type Render } from 'svelte-canvas';

    type UUID = string;

    let width: number;
    let height: number;

    interface Vector {
        x: number;
        y: number;
    }

    interface Sprout {
        id: UUID;
        position: Vector;
        connections: UUID[];
    }

    function newSprout(position: Vector = { x: 0, y: 0 }) {
        return {
            id: crypto.randomUUID(),
            position,
            connections: [],
        }
    }

    function vec(x: number, y: number): Vector {
        return { x, y };
    }

    let sprouts: Sprout[] = [
        newSprout(vec(50, 50)),
        newSprout(vec(100, 100)),
    ]

    let render: Render;
    $: render = ({ context, width, height }) => {
        context.fillStyle = 'white';
        context.fillRect(0, 0, width, height);

        context.fillStyle = 'black';
        sprouts.forEach(sprout => {
            context.beginPath();
            context.arc(sprout.position.x, sprout.position.y, 10, 0, 2 * Math.PI);
            context.fill();
        });
    };
</script>

<svelte:window bind:innerWidth={width} bind:innerHeight={height} />

<Canvas {width} {height}>
    <Layer {render} />
</Canvas>
