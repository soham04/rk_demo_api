var express = require('express');
var router = express.Router();
var bcrypt = require('bcrypt');
const multer = require('multer')
const upload = multer()
const { body, validationResult } = require('express-validator');
var user_db = require('../models/users');
const { Mongoose } = require('mongoose');
const OTP = require('../models/otp');
const trans_db = require('../models/transaction');
var userMiddle = require('../middleware/apis/userMiddle')
var otpGenerator = require('otp-generator');
const noti_db = require('../models/notification');

const fast2sms = require('fast-two-sms')

// get notifications
router.get('/get_notifications/:id', async function (req, res) {
    let id = req.params.id

    try {
        let user = user_db.find({ _id: id })

        let notifications = user.notifications

        return res.json({ response: true, data: notifications })
    } catch (err) {
        res.send({ response: false, message: err })
    }
});

function AddMinutesToDate(date, minutes) {
    return new Date(date.getTime() + minutes * 60000);
}


/**
 * POST send OTP to desired number
 */
router.post('/send_otp', async function (req, res) {
    console.log(req.body);
    if (req.body.mobile == null || req.body.mobile == '') {
        return res.send("Empty Input")
    }

    sendMessage("hi", req.body.mobile, res)

    function sendMessage(message, number, res) {
        var options = {
            authorization:
                "KodtB4vTRFgVNhL2JiAHjkrpmzYElQyqeZ19Xs6nPfD5W3UCxaZqEWHN53TXkBPldvDU9uYw2fgVpSCs",
            message: message,
            numbers: [number],
        };

        // send this message

        fast2sms
            .sendMessage(options)
            .then((response) => {
                res.send("SMS OTP Code Sent Successfully")
            })
            .catch((error) => {
                res.send("Some error taken place")
            });
    }

});

/**
 * Verify OTP from desired mobile 
 */
router.post('/verify_otp', async function (req, res) {
    await OTP.findOne({ '_id': req.body.otp_id, 'opt': req.body.otp })
        .then(result => {
            if (!result) return res.json({ response: true, postMessage: 'Successfully verified' });
            else {
                return res.json({ response: false, postMessage: 'failed' });
            }
        })
    await OTP.deleteOne({ '_id': req.body.otp_id });
});

/**
 * POST for creating new user account
 */
router.post('/create_user', upload.fields([{ name: 'user_image', maxCount: 1 }]), userMiddle.userEmailAuthSpecificAdmin, body('name').not().isEmpty().withMessage('Name Required'), body('email').isEmail(), body('password').isLength({ min: 6 }).withMessage('Atleast 6 character'), async function (req, res, next) {

    let existing_user;

    existing_user = user_db.findOne({ mobile: req.body.mobile })

    if (existing_user) {
        return res.status(409).send({
            response: false,
            data: "User with same mobile number exist",
            code: 4091
        })
    }

    existing_user = user_db.findOne({ email: req.body.email })

    if (existing_user) {
        return res.status(409).send({
            response: false,
            data: "User with same email ID exist",
            code: 4092
        })
    }


    let pwd = bcrypt.hashSync(req.body.pwd, salt);
    console.log(req.files);
    await user_db.create({
        name: req.body.name,
        email: req.body.email,
        mobile: req.body.mobile,
        passwordHash: pwd,
        user_image: req.files.user_image[0].filename,
        joined: Date.now()
    })
        .then(result => {
            res.status(200).json({
                response: true,
                data: result,
                code: 202
            })
        })
        .catch(err => {
            res.status(500).json({
                error: err
            })
        })

});


// router.post('/login', function (req, res) {

//     const user = user_db.findOne({
//         'email': req.body.email,
//     }, function (err, user) {
//         if (!user) return res.json({ isAuth: false, message: ' Auth failed ,email not found' });
//         bcrypt.compare(req.body.password, user.password, (err, result) => {
//             if (result) {
//                 user.fcmToken = req.body.fcmToken;
//                 res.json({ response: true, data: user })
//                 console.log("It matches!")
//             } else {
//                 res.json({ response: false, postMessage: "Incorrect Password" })
//                 console.log("Invalid password!");
//             }

//         });
//     })


// });

// router.post('/profile/:id', async function (req, res, next) {
//     // let id= req.params.id
//     // const data = await user_db.find({'_id':id}); 
//     user_db.findById(req.params.id)
//         .then(result => {
//             if (!result) return res.json({ response: false, postMessage: 'failed' });
//             else {
//                 result.fcmToken = req.body.fcmToken;
//                 return res.json({ response: true, data: result });
//             }
//         })

// });


/**
 * POST Update user profile
 */
router.post('/update_user/:id', async function (req, res, next) {
    let id = req.params.id;
    console.log(id);
    const user = await user_db.findOne({ _id: id });
    console.log(req.body);
    console.log(user);

    if (req.body.name) { user.name = req.body.name }

    if (req.body.email) {
        user.email = req.body.email
    }

    if (req.body.mobile) {
        user.mobile = req.body.mobile[0]
    }
    console.log(user);

    if (req.body.pwd) {
        let pwd = bcrypt.hashSync(req.body.pwd, salt);

        user.pwd = pwd
    }
    console.log(user);

    // user.subscriptions.push({
    //     active: true,
    //     subscription_type: 1,  // 1 - video | 2 - course | 3 - full content
    //     content_id: '63398f3e3490f2fc320125d7',
    //     subscription_start: Date.now(),
    //     subscription_end: Date.now(),
    // })

    console.log(user);

    user.save()




});
router.post('/add_transaction/:_id', async function (req, res) {

    const user = new trans_db({
        "user_id": req.params._id,
        "amount": req.body.amount,
        "date": new Date(),
    });
    if (!user) return res.json({ response: false, postMessage: 'failed' });
    else {
        const data = await user_db.findOne({ '_id': req.params._id });
        return res.json({ response: true, data: data });
    }
})
router.get('/get_transaction/:_id', async function (req, res) {
    trans_db.find({ 'user_id': req.params._id })
        .then(result => {
            if (!result) return res.json({ response: false, postMessage: 'failed' });
            else {
                return res.json({ response: true, data: result });
            }
        })
})
router.post('/update_token/:id', async function (req, res) {
    const user = await user_db.findByIdAndUpdate({ '_id': req.params.id },
        {
            fcmToken: req.body.fcmToken
        });
    if (!user) return res.json({ response: false, postMessage: 'failed' });
    else {
        const data = await user_db.findOne({ '_id': req.params.id });
        return res.json({ response: true, data: data });
    }
});

module.exports = router;
