<script>
    import { onMount } from 'svelte';
      
      export let codeValue;
      export let squareSize; 
      
    let qrcode;
    /*
      onMount(() => {
  
        let script = document.createElement('script');
        script.src = "https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"
        document.head.append(script);
        
            script.onload = function() {
    
                qrcode = new QRCode("qrcode", {
                    text: codeValue,
                        width: squareSize,
                        height: squareSize,
                        colorDark : "#000000",
                        colorLight : "#ffffff",
                        correctLevel : QRCode.CorrectLevel.H
                });		
                
            };				
            
        });*/
    </script>
  
  <style>
    #qrcode {
      width:200px;
      height:200px;
      display: flex;
      align-items: center;
      justify-content: center;
      margin-left: auto;
      margin-right:auto;
    }
  </style>
  
  <!--- <div id="qrcode"></div> --->
  <img src="https://api.qrserver.com/v1/create-qr-code/?size=200x200&data={codeValue}"> 
  
  