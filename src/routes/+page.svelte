<script>
	import { onMount } from 'svelte';
	import JSZip from 'jszip';
	let zip = new JSZip();

	let mainText = `
	ॐ ह्रीं अर्हं अमृताय नमः
।।२५७।।
ॐ ह्रीं अर्हं हविषे नमः
।।२५८।।
ॐ ह्रीं अर्हं व्योममूर्त्तये नमः
।।२५९।।
ॐ ह्रीं अर्ह अमूर्तात्मने नमः
।।२६०।।
ॐ ह्रीं अर्हं निर्लेपाय नमः
।।२६१।।
ॐ ह्रीं अर्हं निर्मलाय नमः
।।२६२।।
ॐ ह्रीं अर्हं अचलाय नमः
।।२६३।।
ॐ ह्रीं अर्हं सोममूर्त्तये नमः
।।२६४।।
ॐ ह्रीं अर्हं सुसौम्यात्मने नमः
।।२६५।।
ॐ ह्रीं अर्हं सूर्यमूर्त्तये नमः
।।२६६।।
ॐ ह्रीं अर्हं महाप्रभाय नमः
।।२६७।।
ॐ ह्रीं अर्हं मन्त्रविदे नमः
।।२६८।।
ॐ ह्रीं अर्हं मन्त्रकृते नमः
।।२६९।।
ॐ ह्रीं अर्हं मन्त्रिणे नमः
।।२७० ।।
ॐ ह्रीं अर्हं मन्त्रमूर्त्तये नमः
।।२७१।।
ॐ ह्रीं अर्हं अनन्तगाय नमः
।।२७२।।
ॐ ह्रीं अर्हं स्वतन्त्राय नमः
।।२७३।।
ॐ ह्रीं अर्हं तन्त्रकृते नमः
।।२७४।।
ॐ ह्रीं अर्हं स्वन्ताय नमः
।।२७५।।
ॐ ह्रीं अर्हं कृतान्तान्ताय नमः
।।२७६।।
ॐ ह्रीं अर्हं कृतान्तकृते नमः
।।२७७।।
ॐ ह्रीं अर्हं कृतिने नमः
।।२७८।।
ॐ ह्रीं अर्हं कृतार्थाय नमः
।।२७९।।
ॐ ह्रीं अर्हं सत्कृत्याय नमः
।।२८०।।
ॐ ह्रीं अर्हं कृतकृत्याय नमः
।।२८१।।
ॐ ह्रीं अर्हं कृतक्रतवे नमः
।।२८२।।
ॐ ह्रीं अर्हं नित्याय नमः
।।२८३।।
ॐ ह्रीं अर्हं मृत्युञ्जयाय नमः
।।२८४।।
ॐ ह्रीं अर्हं अमृत्यवे नमः
।।२८५।।
ॐ ह्रीं अर्हं अमृतात्मने नमः
।।२८६।।
ॐ ह्रीं अर्हं अमृतोद्भवाय नमः
।।२८७।।
ॐ ह्रीं अर्हं ब्रह्मनिष्ठाय नमः
।।२८८।।
ॐ ह्रीं अर्हं परंब्रह्मणे नमः
।।२८९।।
ॐ ह्रीं अर्हं ब्रह्मात्मने नमः
।।२९० ।।


	`;

	mainText = mainText.trim();
	mainText += '\n'; //To make sure the last text can be split
	mainText = mainText.replace(/॥/g, '।।');
	let splitTextList = mainText.split(/।।\n/g);
	// console.log(splitTextList[0], splitTextList[2], splitTextList[29]);
	for (let i = 0; i < splitTextList.length; i++) {
		splitTextList[i] = splitTextList[i].replace(/[\r\n\x0B\x0C\u0085\u2028\u2029]+/g, ' ') + '।।';
	}
	// This fixes the last
	splitTextList.pop();
	console.log(splitTextList.length + ' files');

	let canvasList = [];
	onMount(() => {
		for (let i = 0; i < canvasList.length; i++) {
			t2i(canvasList[i], splitTextList[i]);

			canvasList[i].toBlob(
				(blob) => {
					// If zip functionality is not needed, direct image download can be done using
					// the following code and removing all the code for dlBtn element.

					// const anchor = document.createElement('a');
					// anchor.download = 'image' + i + '.jpg'; // optional, but you can give the file a name
					// anchor.href = URL.createObjectURL(blob);
					// anchor.click(); //Magic
					// URL.revokeObjectURL(anchor.href); // remove it from memory and save on memory! 😎
					let fileName = 'image' + (i + 1) + '.jpg'; // optional, but you can give the file a name
					zip.file(fileName, blob);
				},
				'image/jpeg',
				0.9
			);
		}
		// comment the following to removing zip functionality.
		let dlBtn = document.getElementById('dlBtn');
		dlBtn.addEventListener('click', download);
	});
	async function download() {
		const content = await zip.generateAsync({ type: 'blob' });
		const anchor = document.createElement('a');
		anchor.download = 'images.zip';
		anchor.href = URL.createObjectURL(content);
		anchor.click(); //Auto click the link
		URL.revokeObjectURL(anchor.href); // remove it from memory and save on memory!
	}

	function t2i(textCanvas, text) {
		let ctx = textCanvas.getContext('2d');
		ctx.reset();
		ctx.textBaseline = 'middle';
		ctx.textAlign = 'center';
		ctx.font = '40px Arial';
		ctx.fillStyle = 'rgb(0,0,0)';
		ctx.fillRect(0, 0, 650, 200);
		ctx.fillStyle = 'rgb(255,255,255)';
		ctx.fillText(text, textCanvas.width / 2, textCanvas.height / 2);
	}
</script>

<h3>Canvas</h3>
<div>
	<button id="dlBtn">Download ZIP</button>
</div>
{#each splitTextList as tl, i}
	<canvas height="200" width="650" bind:this={canvasList[i]} />
{/each}
