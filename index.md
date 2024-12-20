<!DOCTYPE HTML5>
<head>
<title>lokumnin-index-html</title>
</head>
<!--meta http-equiv="refresh"
        content="8; url = https://lokumnin.github.io/lokumnin"-->  
<meta content="text/html; charset=utf-8" http-equiv="Content-Type" />
<meta name="viewport" content="width=device-width,initial-scale=1">
<meta name="Author" content="Dr. Muraleedharan Koluthappallil">
<meta name="website" content="https://lokumnin.github.io">
<link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
<link rel="stylesheet" href="/global.css" type="text/css">
<script src="https://www.w3schools.com/lib/w3.js"></script>
</head>
<body bgcolor="white" class="w3-container w3-color-black w3-white w3-left">
<section class="page-header logo">
<header>
<div class="w3-logo-image">
<H1><a class="anchor" href="https://github.com/lokumnin">
<img src='./Logo_LOKUMNIN.jpg' class="w3-center w3-img" width="" height=""></a>
</H1>
</div>
<section class="w3-nav">
<a class="w3-logo w3-green w3-btn w3-left" href="/index.html">home</a>
<a class="w3-logo w3-blue w3-btn w3-left" href="/Author.html">author</a>
<a class="w3-logo w3-red w3-btn w3-left" href="https://github.com/kolumnin/">github</a>
<a class="w3-logo w3-yellow w3-btn w3-left" href="https://kolumnin.awsapps.com/start/">aws</a>
<a class="w3-logo w3-orange w3-btn w3-left" href="https://kolumnin.hashnode.dev">blog</a>
<a class="w3-logo w3-pink w3-btn w3-left" href="https://sites.google.com/site">google</a>
<a class="w3-logo w3-teal w3-btn w3-left" href="https://www.microsoft.com">microsoft</a>
<a class="w3-logo w3-yellow w3-btn w3-left" href="https://www.yahoo.com">yahoo</a>
<a class="w3-logo w3-purple w3-btn w3-left" href="https://www.bsnl.co.in">telecom</a>
<a class="w3-logo w3-green w3-btn w3-left" href="https://gitlab.com/users/sign_in">gitlab</a>
</section>
</header>
<section>
</section>
<section>
<hr>
If you Sponsor my open source activities, you will be recognised as my sponsor anywhere I go. You will also get a sponsor badge.
<iframe src="https://github.com/sponsors/kolumnin/button" title="Sponsor kolumnin" height="35" width="116" style="border: 0;"></iframe>
<iframe src="https://github.com/sponsors/lokumnin/card" title="Sponsor lokumnin" height="225" width="600" style="border: 0;"></iframe>
<div id="smart-button-container">
    <div style="text-align: center"><label for="description">Sponsor </label><input type="text" name="descriptionInput" id="description" maxlength="127" value=""></div>
      <p id="descriptionError" style="visibility: hidden; color:red; text-align: center;">Please enter a description</p>
    <div style="text-align: center"><label for="amount"> </label><input name="amountInput" type="number" id="amount" value="" ><span> USD</span></div>
      <p id="priceLabelError" style="visibility: hidden; color:red; text-align: center;">Please enter a price</p>
    <div id="invoiceidDiv" style="text-align: center; display: none;"><label for="invoiceid"> </label><input name="invoiceid" maxlength="127" type="text" id="invoiceid" value="" ></div>
      <p id="invoiceidError" style="visibility: hidden; color:red; text-align: center;">Please enter an Invoice ID</p>
    <div style="text-align: center; margin-top: 0.625rem;" id="paypal-button-container"></div>
  </div>
<div class="w3-rest">
  <script src="https://www.paypal.com/sdk/js?client-id=sb&enable-funding=venmo&currency=USD" data-sdk-integration-source="button-factory"></script>
  <script>
  function initPayPalButton() {
    var description = document.querySelector('#smart-button-container #description');
    var amount = document.querySelector('#smart-button-container #amount');
    var descriptionError = document.querySelector('#smart-button-container #descriptionError');
    var priceError = document.querySelector('#smart-button-container #priceLabelError');
    var invoiceid = document.querySelector('#smart-button-container #invoiceid');
    var invoiceidError = document.querySelector('#smart-button-container #invoiceidError');
    var invoiceidDiv = document.querySelector('#smart-button-container #invoiceidDiv');

    var elArr = [description, amount];

    if (invoiceidDiv.firstChild.innerHTML.length > 1) {
      invoiceidDiv.style.display = "block";
    }

    var purchase_units = [];
    purchase_units[0] = {};
    purchase_units[0].amount = {};

    function validate(event) {
      return event.value.length > 0;
    }

    paypal.Buttons({
      style: {
        color: 'gold',
        shape: 'rect',
        label: 'paypal',
        layout: 'vertical',
        
      },

      onInit: function (data, actions) {
        actions.disable();

        if(invoiceidDiv.style.display === "block") {
          elArr.push(invoiceid);
        }

        elArr.forEach(function (item) {
          item.addEventListener('keyup', function (event) {
            var result = elArr.every(validate);
            if (result) {
              actions.enable();
            } else {
              actions.disable();
            }
          });
        });
      },

      onClick: function () {
        if (description.value.length < 1) {
          descriptionError.style.visibility = "visible";
        } else {
          descriptionError.style.visibility = "hidden";
        }

        if (amount.value.length < 1) {
          priceError.style.visibility = "visible";
        } else {
          priceError.style.visibility = "hidden";
        }

        if (invoiceid.value.length < 1 && invoiceidDiv.style.display === "block") {
          invoiceidError.style.visibility = "visible";
        } else {
          invoiceidError.style.visibility = "hidden";
        }

        purchase_units[0].description = description.value;
        purchase_units[0].amount.value = amount.value;

        if(invoiceid.value !== '') {
          purchase_units[0].invoice_id = invoiceid.value;
        }
      },

      createOrder: function (data, actions) {
        return actions.order.create({
          purchase_units: purchase_units,
        });
      },

      onApprove: function (data, actions) {
        return actions.order.capture().then(function (orderData) {

          // Full available details
          console.log('Capture result', orderData, JSON.stringify(orderData, null, 2));

          // Show a success message within this page, e.g.
          const element = document.getElementById('paypal-button-container');
          element.innerHTML = '';
          element.innerHTML = '<h3>Thank you for your payment!</h3>';

          // Or go to another URL:  actions.redirect('thank_you.html');
          
        });
      },

      onError: function (err) {
        console.log(err);
      }
    }).render('#paypal-button-container');
  }
  initPayPalButton();
  </script>
<hr>      
<script src="https://forms.app/static/embed.js" type="text/javascript" async defer onload="new formsapp('6625de41cc6abcec7f0dd66b', 'popover', {'button':{'color':'#ff9e24'},'align':'right','autoOpen':{'action':'aftersettime','setTimeSeconds':'20'}});"></script>
<hr>
<footer Id="Logo" class="w3-col-3 w3-footer w3-left">
<div>©2000-<a href="http://KoluMnIN.github.io/Author.html">Dr.K.Muraleedharan</a> All rights reserved.</div>
<p>Last update: 10:40 2024 August 25</a>@kolumnin</p>
</footer>
</section>
<div class="w3-col-3 w3-pale-green w3-center"><a href="https://GitHub.com/kolumnin"><b>KoluMnIN</b></a> is hosted by <a href="https://GitHub.com"><b>GitHub</b></a>
</div>
</div>
</div>
</body>
<script type="module">
  // Import the functions you need from the SDKs you need
  import { initializeApp } from "https://www.gstatic.com/firebasejs/9.10.0/firebase-app.js";
  import { getAnalytics } from "https://www.gstatic.com/firebasejs/9.10.0/firebase-analytics.js";
  // TODO: Add SDKs for Firebase products that you want to use
  // https://firebase.google.com/docs/web/setup#available-libraries

  // Your web app's Firebase configuration
  // For Firebase JS SDK v7.20.0 and later, measurementId is optional
  const firebaseConfig = {
    apiKey: "AIzaSyAlW-BISs3BXUuBIIP_yFATfF86wRjyAUk",
    authDomain: "kolumnin-a9140.firebaseapp.com",
    projectId: "kolumnin",
    storageBucket: "kolumnin.appspot.com",
    messagingSenderId: "750305128307",
    appId: "1:750305128307:web:3c0e47eecba67f5806df06",
    measurementId: "G-9CWLGQNW5G"
  };

  // Initialize Firebase
  const app = initializeApp(firebaseConfig);
  const analytics = getAnalytics(app);
</script>
</html>

