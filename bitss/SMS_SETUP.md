# SMS OTP Setup Guide for BITS Student Portal

## Setup Instructions:

### Option 1: Using Twilio (Recommended)

1. **Create Twilio Account:**
   - Go to https://www.twilio.com/
   - Sign up for a free account
   - Get your Account SID and Auth Token
   - Get a Twilio phone number

2. **Install Twilio SDK:**
   ```bash
   npm install twilio
   ```

3. **Replace the sendSMSOTP function:**
   ```javascript
   function sendSMSOTP(phoneNumber, otp) {
       const accountSid = 'YOUR_TWILIO_ACCOUNT_SID';
       const authToken = 'YOUR_TWILIO_AUTH_TOKEN';
       const client = require('twilio')(accountSid, authToken);
       const studentName = document.getElementById('firstName').value + ' ' + document.getElementById('lastName').value;
       
       client.messages
           .create({
               body: `BITS Institute: Hello ${studentName}! Your OTP is ${otp}. This code expires in 10 minutes.`,
               from: 'YOUR_TWILIO_PHONE_NUMBER',
               to: phoneNumber
           })
           .then(message => {
               console.log('SMS sent successfully:', message.sid);
               showSMSSuccess(phoneNumber);
           })
           .catch(error => {
               console.error('SMS failed:', error);
               showSMSFallback(otp);
           });
   }
   ```

### Option 2: Using Nexmo/Vonage

1. **Create Vonage Account:**
   - Go to https://www.vonage.com/
   - Sign up and get API key and secret

2. **Replace sendSMSOTP function:**
   ```javascript
   function sendSMSOTP(phoneNumber, otp) {
       const apiKey = 'YOUR_VONAGE_API_KEY';
       const apiSecret = 'YOUR_VONAGE_API_SECRET';
       const vonage = new Vonage({ apiKey, apiSecret });
       const studentName = document.getElementById('firstName').value + ' ' + document.getElementById('lastName').value;
       
       const from = 'BITSINST';
       const to = phoneNumber;
       const text = `BITS Institute: Hello ${studentName}! Your OTP is ${otp}. This code expires in 10 minutes.`;
       
       vonage.message.sendSms(from, to, text, (err, responseData) => {
           if (err) {
               console.log('SMS failed:', err);
               showSMSFallback(otp);
           } else {
               if (responseData.messages[0]['status'] === '0') {
                   console.log('SMS sent successfully');
                   showSMSSuccess(phoneNumber);
               } else {
                   console.log('SMS failed:', responseData.messages[0]['error-text']);
                   showSMSFallback(otp);
               }
           }
       });
   }
   ```

### Option 3: Using Local SMS Gateway

For local Indian numbers, you can use services like:
- MSG91
- TextLocal
- SendinBlue
- Fast2SMS

Example with MSG91:
```javascript
function sendSMSOTP(phoneNumber, otp) {
    const authKey = 'YOUR_MSG91_AUTH_KEY';
    const studentName = document.getElementById('firstName').value + ' ' + document.getElementById('lastName').value;
    const message = `BITS Institute: Hello ${studentName}! Your OTP is ${otp}. This code expires in 10 minutes.`;
    
    fetch(`https://api.msg91.com/api/sendhttp.php?authkey=${authKey}&mobiles=${phoneNumber}&message=${encodeURIComponent(message)}&sender=BITSIN&route=4&country=91`)
        .then(response => response.text())
        .then(result => {
            console.log('SMS sent:', result);
            showSMSSuccess(phoneNumber);
        })
        .catch(error => {
            console.error('SMS failed:', error);
            showSMSFallback(otp);
        });
}
```

### Helper Functions to Add:

```javascript
function showSMSSuccess(phoneNumber) {
    document.getElementById('otpAlert').style.display = 'block';
    document.getElementById('otpAlert').innerHTML = '<i class="fas fa-check-circle"></i> OTP sent to ' + phoneNumber + '! Please check your messages.';
    document.getElementById('otpSection').style.display = 'block';
    document.getElementById('emailVerificationSection').style.display = 'none';
    setTimeout(() => {
        document.getElementById('otpAlert').style.display = 'none';
    }, 5000);
}

function showSMSFallback(otp) {
    document.getElementById('errorAlert').style.display = 'block';
    document.getElementById('errorAlert').innerHTML = '<i class="fas fa-exclamation-circle"></i> SMS service unavailable. Demo OTP: ' + otp;
    document.getElementById('otpSection').style.display = 'block';
    document.getElementById('emailVerificationSection').style.display = 'none';
    setTimeout(() => {
        document.getElementById('errorAlert').style.display = 'none';
    }, 8000);
}
```

### Test Numbers:
- Use verified numbers during development
- Twilio provides test credentials
- Local services have sandbox numbers

### Phone Number Format:
- For Indian numbers: +91xxxxxxxxxx
- Validate number format before sending
- Add country code detection

## Pricing Information:
- Twilio: ~$0.0075 per SMS (varies by country)
- Nexmo: ~$0.0053 per SMS 
- Indian services: ₹0.15-0.35 per SMS

Choose service based on your target region and budget requirements.