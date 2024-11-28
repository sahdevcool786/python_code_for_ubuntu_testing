from http.server import HTTPServer, BaseHTTPRequestHandler
import ssl

cipher = 'NULL-SHA256,AES128-SHA256,AES256-SHA256,AES128-GCM-SHA256,AES256-GCM-SHA384,DH-RSA-AES128-SHA256,DH-RSA-AES256-SHA256,DH-RSA-AES128-GCM-SHA256,DH-RSA-AES256-GCM-SHA384,DH-DSS-AES128-SHA256,DH-DSS-AES256-SHA256,DH-DSS-AES128-GCM-SHA256,DH-DSS-AES256-GCM-SHA384'

context = ssl.create_default_context(ssl.Purpose.CLIENT_AUTH)
context.load_cert_chain(certfile="certificate.pem", 
                        keyfile="key.pem")
# Remove the line below to enable TLS 1.3
context.options = ssl.OP_NO_TLSv1_3
context.set_ciphers(cipher)

httpd = HTTPServer(('192.168.1.6', 47333), BaseHTTPRequestHandler)

httpd.socket = context.wrap_socket (httpd.socket, server_side=True)

httpd.serve_forever()
