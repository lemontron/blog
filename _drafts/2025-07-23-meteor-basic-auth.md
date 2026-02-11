``` javascript
import { DDP } from 'meteor/ddp-client';
import crypto from 'crypto';
import basicAuth from 'express-basic-auth';

const ddp = DDP.connect(Meteor.absoluteUrl());

const hashPassword = (password) => ({
	'digest': crypto.createHash('sha256').update(password).digest('hex'),
	'algorithm': 'sha-256',
});

export const authMiddleware = basicAuth({
	'authorizeAsync': true,
	'challenge': true,
	'authorizer': (email, password, cb) => {
		ddp.call('login', {
			'user': {'email': email},
			'password': hashPassword(password),
		}, (err, result) => {
			console.log('result:', result, 'err:', err);
			return cb(null, !!result);
		});
	},
});
```