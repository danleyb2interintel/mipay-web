# MIPAY Web Sdk
### Quick Setup

#### Checkout

```html

    // inside an activity
    // this will open the MIPAY app on on mobile devices and if the MIPAY app installed else opens a webpage for the payment options
    var reference  = "1-INTINT-CVI35";

    // ...
    
```

To get the result, you need to handle the intent
```javascript

<!DOCTYPE html>
<head>
<title>main</title>
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
<script>
window.addEventListener("message", function(ev) {
    if (ev.data.message === "deliverResult") {
        ev.source.close();
	if(ev.data.success){
		var p = ev.data.payload;
		console.log(p);
		console.log(JSON.stringify(p));
		var div = 'Transaction Successful</b>';
		for(i in p){
			if(i === 'response'){
				for(j in p[i]){
				div = div + 'Response: <div>'+j+': <b>'+p[i][j]+'</b></div>';
					}
			
			}else{
				div = div + '<div>'+i+': <b>'+p[i]+'</b></div>';
				}
			}
		document.getElementById('result').innerHTML = div

		}
	else{
		var div = 'Transaction Failed</b>';
		document.getElementById('result').innerHTML = div

		}
    }
});

function Go() {
var w = 600;
var h = 600;

var left = (window.screen.width / 2) - ((w / 2) + 10);
var top = (window.screen.height / 2) - ((h / 2) + 50);
    var url = 'https://gomipay.com/checkout/';
    var reference  = "1-INTINT-CVI35";

    var child = window.open(url +"?reference="+reference+"", "_blank","toolbar=no, location=no, directories=no, status=no, menubar=no, scrollbars=yes, resizable=no, copyhistory=no, width="+w+", height="+h+", top="+top+", left="+left);
    var interval = setInterval(function() {

	child.postMessage({ message: "requestResult" }, url);

            if (child.closed) {
		clearInterval(interval);
            }
	console.log('Interval Check')
    }, 500);
}
</script>
</head>
<body>
<button onclick="Go()">Go</button>
<div>
<p>Result</p>
<div id="result"></div>
</div>
</body>


```
