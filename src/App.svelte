<script>
	import {
		scaleLinear,
		extent,
		line,
		area,
		curveCatmullRom as curve,
	} from 'd3';

	import data from './data.json';

	let width, height;

	$: xScale = scaleLinear()
		.domain(extent(data, (d) => d.xValue))
		.range([0, width || 1]);

	$: yScale = scaleLinear()
		.domain(extent(data, (d) => d.yValue))
		.range([height || 1, 0]);

	$: lineGenerator = line()
		.x((d) => xScale(d.xValue))
		.y((d) => yScale(d.yValue))
		.curve(curve);

	$: areaGenerator = area()
		.x((d) => xScale(d.xValue))
		.y0(height)
		.y1((d) => yScale(d.yValue))
		.curve(curve);
</script>

<main
	bind:clientWidth={width}
	bind:clientHeight={height}
>
	<svg
		width={width}
		height={height}
	>
		<path
			class="area"
			d={areaGenerator(data)}
		/>
		<path
			class="line"
			d={lineGenerator(data)}
		/>
		<rect
			x="0"
			y="0"
			width={width}
			height={height}
		/>
	</svg>
</main>

<style>
	path.line {
		stroke: #bbb;
		fill: none;
		stroke-width: 2px;
	}

	path.area {
		fill: #fe7aae;
	}

	rect {
		fill: var(--backgroundColor);
		transform: translateX(100%);
		animation: 1s ease-out 0s 1 slideRect;
	}

	@keyframes slideRect {
		0% {
			transform: translateX(0);
		}
		100% {
			transform: translateX(100%);
		}
	}
</style>
