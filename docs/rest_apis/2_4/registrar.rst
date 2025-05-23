Registrar
~~~~~~~~~

.. http:get::  /v2.4/agents/

    Get ordered list of registered agents

    **Example response:**

    .. sourcecode:: json

        {
          "code": 200,
          "status": "Success",
          "results": {
            "uuids": [
              "5e600bce-a5cb-4f5a-bf08-46d0b45081c5",
              "6dab10e4-6619-4ff9-9062-ee6ad23ec24d",
              "d432fbb3-d2f1-4a97-9ef7-75bd81c00000"
            ]
          }
        }

    :>json int code: HTTP status code
    :>json string status: Status as string
    :>json object results: Results as a JSON object
    :>json list uuids: List of registered agents


.. http:get::  /v2.4/agents/{agent_id:UUID}

    Get EK certificate, AIK and optinal contact ip and port of agent `agent_id`.

    **Example response:**

    .. sourcecode:: json

        {
          "code": 200,
          "status": "Success",
          "results": {
            "aik_tpm": "ARgAAQALAAUAcgAAABAAFAALCAAAAAAAAQDjZ4J2HO7ekIONAX/eYIzt7ziiVAqE/1D7I9oEwIE88dIfqH0FQLJAg8u3+ZOgsJDQr9HiMhZRPhv8hRuia8ULdAomyOFA1cVzlBF+xcPUEemOIofbvcBNAoTY/x49r8LpqAEUBBiUeOniQbjfRaV2S5cEAA92wHLQAPLF9Sbf3zNxCnbhtRkEi6C3NYl8/FJqyu5Z9vvwEBBOFFTPasAxMtPm6a+Z5KJ4rDflipfaVcUvTKLIBRI7wkuXqhTR8BeIByK9upQ3iBo+FbYjWSf+BaN+wodMNgPbzxyL+tuxVqiPefBbv+sTWVxmYfo5i84FlbNOAW3APH8c+jZ3tgbt",
            "ek_tpm": "AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEA0YwlPPIoXryMvbD5cIokN9OkljL2mV1oDxy7ETBXBe1nL9OWrLNO8Nbf8EaSNCtYCo5iqCwatnVRMPqNXcX8mQP0f/gDAqXryb+F192IJLKShHYSN32LJjCYOKrvNX1lrmr377juICFSRClE4q+pCfzhNj0Izw/eplaAI7gq41vrlnymWYGIEi4McErWG7qwr7LR9CXwiM7nhBYGtvobqoaOm4+f6zo3jQuks/KYjk0BR3mgAec/Qkfefw2lgSSYaPNl/8ytg6Dhla1LK8f7wWy/bv+3z7L11KLr8DZiFAzKBMiIDfaqNGYPhiFLKAMJ0MmJx63obCqx9z5BltV5YQ==",
            "ekcert": "-----BEGIN CERTIFICATE----- (...) -----END CERTIFICATE-----",
            "mtls_cert": "-----BEGIN CERTIFICATE----- (...) -----END CERTIFICATE-----",
            "ip": "127.0.0.1",
            "port": 9002,
            "regcount": 1
          }
        }

    :>json int code: HTTP status code
    :>json string status: Status as string
    :>json object results: Results as a JSON object
    :>json string aik_tpm: base64 encoded AIK. The AIK format is TPM2B_PUBLIC from tpm2-tss.
    :>json string ek_tpm: base64 encoded EK. When a `ekcert` is submitted it will be the public key of that certificate.
    :>json string ekcert: PEM encoded EK certificate chain. Gets extracted from NV `0x1c00002` (ekcert) + `[0x01c00100..0x01c001ff]` (ek ca chain).
    :>json string mtls_cert: Agent HTTPS server certificate. PEM encoded.
    :>json string ip: IPv4 address for contacting the agent. Might be `null`.
    :>json integer port: Port for contacting the agent. Might be `null`.


.. http:post::  /v2.4/agents/{agent_id:UUID}

    Add agent `agent_id` to registrar.

    **Example request**:

    .. sourcecode:: json

        {
          "ekcert": "-----BEGIN CERTIFICATE----- (...) -----END CERTIFICATE-----",
          "aik_tpm": "ARgAAQALAAUAcgAAABAAFAALCAAAAAAAAQCg5mMzNFqdlUbW8uI/GuMcIIvOXXTohHFTas59JlwrJQVed+5klWP+j7tI7492YPmCnoZvP4T4YdT1PN7tHHGfF81AeMnuw5GV5RkW/QeSD+ssB4f6AfuzYJgBkc28zKmpRRHUbwN4rb/HnJgRXdXsuIcnOqGcC39pD0kiu5TrN6hekjxTQtfAbIlQwwDwHCxKWdtH5x7avd15hqc6cBc2gjTQksXrk+OiMwOFTJ68n0qY+dQYuBTjE66YXn9S8cdU9sbjCTSdLRqFEpAyfkSV8F2An7N3DWNIA+PW/mVmd8XhPeYUoMlweXBOwc3e9zM9lZmMvregrFHKYc7CXChz",
          "mtls_cert": "-----BEGIN CERTIFICATE----- (...) -----END CERTIFICATE-----",
          "ip": "127.0.0.1",
          "port": "9002"
        }


    :<json string ekcert: PEM encoded EK certificate chain. Gets extracted from NV `0x1c00002` (ekcert) + `[0x01c00100..0x01c001ff]` (ek ca chain).
    :<json string aik_tpm: base64 encoded AIK. The AIK format is TPM2B_PUBLIC from tpm2-tss.
    :<json string mtls_cert: Agent HTTPS server certificate. PEM encoded.
    :<json string ip: (Optional) contact IPv4 address for the verifier and tenant to use.
    :<json string port: (Optional) contact port for the verifier and tenant to use.

    **Example response**:

    .. sourcecode:: json

        {
          "code": 200,
          "status": "Success",
          "results": {
            "blob": "utzA3gAAAAEARAAgC/w9LP1PKZ9thEk+GkMg4m+tkc9TkavcvFiFL6xbXM2q2fTRyKmQnxuCJc0tQdgsRXMftGiKJyA/SUo8kGNVmcNfAQCs79kl9Ir49JJ8rfyMfDIqOuSVlu9PhxGUOeVzAdxyUmPxq5Qp0s431n/KeL/5nUaVXC+qpOftF4bmVtXwLGTTUbKtyT3GG+9ujkjiwHCQhSKTQ8HiuARgXXh13ntFsJ75PBD5dWauLTuciYZI/WQDVXAcgMnQNxodJUi9ir1GxJWz8zufjVQTVjrlgsgeBdOKbB6+H81K1d9prWhZaVLP+wIwO3YuWgtNHNi90E1z/dah2pzfUpLvJo3lNZ4bJgrJUR507AokGKIFm7EfOf+5WWWAvGxGtgqTJB27vgE0CVBLEuDUHoRcLVBi1Np4GGNTByalxbulg8x1eGtZyuQF"
          }
        }

    :>json int code: HTTP status code
    :>json string status: Status as string
    :>json object results: Results as a JSON object
    :>json string blob: base64 encoded blob containing the `aik_tpm` name and a challenge. Is encrypted with `ek_tpm`.


.. http:delete::  /v2.4/agents/{agent_id:UUID}

    Remove agent `agent_id` from registrar.

    **Example response**:

    .. sourcecode:: json

        {
          "code": 200,
          "status": "Success",
          "results": {}
        }

    :>json int code: HTTP status code
    :>json string status: Status as string
    :>json object results: Results as a JSON object (empty)


.. http:put::  /v2.4/agents/{agent_id:UUID}/activate

    Activate physical agent `agent_id`

    **Example request:**

    .. sourcecode:: json

        {
          "auth_tag": "7087ba88746886262de743587ed97aea6b6e3f32755de5d85415c40feef3169bc58d38855ddb96e32efdd8745d0bdfef"
        }


    :<json string auth_tag: hmac containing the challenge from `blob` and the `agent_id`.


    **Example response**:

    .. sourcecode:: json

        {
          "code": 200,
          "status": "Success",
          "results": {}
        }

    :>json int code: HTTP status code
    :>json string status: Status as string
    :>json object results: Results as a JSON object (empty)

.. http:get::  /version

    Get current and supported API versions

    **Example request:**

    .. sourcecode:: http

       GET /version HTTP/1.1
       Host: example.com
       Accept: application/json


    **Example response:**

    .. sourcecode:: json

       {
         "code": 200,
         "status": "Success",
         "results": {
           "current_version": "2.4",
           "supported_versions": [
             "1.0",
             "2.0",
             "2.1",
             "2.2",
             "2.3",
             "2.4"
           ]
         }
       }

    :>json int code: HTTP status code
    :>json string status: Status as string
    :>json object results: Results as a JSON object
    :>json string current_version: Current API version
    :>json list supported_versions: List of supported API versions

