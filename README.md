jasvet
======

jasvet signs messages using Bitcoin keys in three formats.


Message to be signed and clear texts are formatted with RFC2440 rules.
The base64 signatures are signature data encoded in base64. These data are the leading byte (between 27 and 34), the r value, the s value, and, if the signed message is included, it is concatenated at the end (ie byte 65 and following).
CRC24 is calculated over signature data.


Usage:

	verifySignature(addr, b64sig, msg):
		addr   = '1BCwRkTsYzK5aNK4sdF7Bpti3PhrkPtLc4'
		b64sig = 'HJAicnCa0DQ3xAQl...'
		msg    = 'Hello world!'

		Does not return anything, raises exception if signature not valid


	ASvX(privkey, msg):       X in [0, 1CS, 1B64]
		privkey = '\x01'*32, ie 32-bytes long string
		msg     = 'Hello world!'

		Returns:
			v0     Bitcoin signature
			v1CS   ASCII armored clearsign
			v1B64  ASCII armored base64


Output formats of ASvX:

	Version 0 (Bitcoin compatible)
		base64( leading byte + r + s )

	Version 1 ClearSign (indented by two tabs)
		-----BEGIN BITCOIN MESSAGE-----
		
		Text signed
		-----BEGIN BITCOIN SIGNATURE-----
		base64( leading byte + r + s )
		=base64( CRC24( leading byte + r + s ) )
		-----END BITCOIN SIGNATURE-----

	Version 1 Base64 (indented by two tabs)
		-----BEGIN BITCOIN SIGNED MESSAGE-----
		base64( leading byte + r + s + msg )
		=base64( CRC24( leading byte + r + s + msg ) )
		-----END BITCOIN SIGNED MESSAGE-----


Note:
	Works with Python 2.x, without dependencies
	The leading byte is the first byte of Bitcoin signatures, ie something between 27 and 34
