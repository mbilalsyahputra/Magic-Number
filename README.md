# Magic-Number
&lt;!doctype html> &lt;html lang="en"> &lt;head>     &lt;title>MagicNumber&lt;/title>     &lt;style>         div {             font-family: "Helvetica Neue";             line-height:22px;             font-size:15px;             margin:10px 0;             color: #333;         }         em {             padding: 2px 4px;             background-color: #efefef;             font-style: normal;         }     &lt;/style> &lt;/head>  &lt;body>  &lt;input type="file" id="file-selector">  &lt;div id="files">&lt;/div> &lt;script>     const uploads = []     const fileSelector = document.getElementById('file-selector')     fileSelector.addEventListener('change', (event) => {         console.time('FileOpen')         const file = event.target.files[0]         const filereader = new FileReader()         filereader.onloadend = function(evt) {             if (evt.target.readyState === FileReader.DONE) {                 const uint = new Uint8Array(evt.target.result)                 let bytes = []                 uint.forEach((byte) => {                     bytes.push(byte.toString(16))                 })                 const hex = bytes.join('').toUpperCase()                 uploads.push({                     filename: file.name,                     filetype: file.type ? file.type : 'Unknown/Extension missing',                     binaryFileType: getMimetype(hex),                     hex: hex                 })                 render()             }             console.timeEnd('FileOpen')         }         const blob = file.slice(0, 4);         filereader.readAsArrayBuffer(blob);     })     const render = () => {         const container = document.getElementById('files')         const uploadedFiles = uploads.map((file) => {             return `&lt;div>                     &lt;strong>${file.filename}&lt;/strong>&lt;br>                     Filetype from file object: ${file.filetype}&lt;br>                     Filetype from binary: ${file.binaryFileType}&lt;br>                     Hex: &lt;em>${file.hex}&lt;/em>                     &lt;/div>`         })         container.innerHTML = uploadedFiles.join('')     }     const getMimetype = (signature) => {         switch (signature) {             case '89504E47':                 return 'image/png'             case '47494638':                 return 'image/gif'             case '25504446':                 return 'application/pdf'             case 'FFD8FFDB':             case 'FFD8FFE0':                 return 'image/jpeg'             case '504B0304':                 return 'application/zip'             default:                 return 'Unknown filetype'         }     } &lt;/script>  &lt;/body> &lt;/html>
