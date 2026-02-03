# BITS Student Portal - SMS OTP Only Setup

## Current Status:

✅ **Phone Number OTP Only** - Email templates removed
✅ **SMS-based verification** - Students receive OTP via SMS
✅ **No email required** - Simplified submission process

## Setup Instructions:

1. **Choose SMS Service Provider:**
   - For international: Twilio, Nexmo
   - For India: MSG91, TextLocal, Fast2SMS
   - Refer to `SMS_SETUP.md` for detailed integration

2. **Get API Credentials:**
   - Sign up for chosen SMS service
   - Obtain API key/secret and sender ID
   - Verify sender ID for your region

3. **Implement SMS Function:**
   - Replace the `sendSMSOTP` function in `student.html`
   - Use examples from `SMS_SETUP.md`

## How It Works:

1. Student enters phone number in form
2. Clicks "Send OTP to Phone"
3. System generates 6-digit OTP
4. OTP sent via SMS to student's phone
5. Student enters OTP to verify
6. Form submission enabled after verification

## Testing:

- Demo mode shows OTP in browser console
- Real implementation sends actual SMS
- Fallback shows OTP if SMS fails
- Check browser console for debugging

## Security Notes:

- OTP expires after 10 minutes
- Phone number validation recommended
- Use HTTPS in production
- Store API keys securely