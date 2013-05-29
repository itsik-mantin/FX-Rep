#!/usr/bin/env python

import SimpleHTTPServer, SocketServer, CGIHTTPServer
import os, random, re
from CGIHTTPServer import CGIHTTPRequestHandler
import logging

mainDir = 'C:\MyFiles\MyWork\Efoxy\JsWare\FX_REP'

formatter = logging.Formatter('%(asctime)s %(levelname)s %(message)s')
logger = logging.getLogger('LOGLOG')
hdlr = logging.FileHandler(os.path.join(mainDir, 'Log\log.log'), mode='w')
logger.addHandler(hdlr)
logger.setLevel(logging.DEBUG)


fpDigests = []

class MyHandler(CGIHTTPRequestHandler):
    def __init__(self, req, client_addr, server):
        CGIHTTPRequestHandler.__init__(self, req, client_addr, server)

    def do_GET(self):
        m = re.match(r'^(?P<Method>\w+)\s+/\?MY_PARAM=(?P<DAT>\S+)\s+.*$', self.requestline)
        if m:
            res = m.groupdict()
            logger.debug('************** (%s) %s **************' % (res['Method'], res['DAT'][:100]))
            fpDigests.append(res['DAT'])
            logger.debug('*** HEADERS: %s ***\n*** HURRAY %d ***' % (repr(self.headers), len(res['DAT'])))
            logger.debug('Got DAT %d bytes' % len(res['DAT']))
            logger.debug('************** %s **************\n' % res['DAT'])
        # Prepare response
        self.send_response(200, "ok")
        self.send_header('Access-Control-Allow-Origin', 'http://www.tnuva.co.il')
        self.send_header('Access-Control-Allow-Methods', 'GET, POST, OPTIONS')
        self.end_headers()

    def do_POST(self):
        self.send_response(200, "ok")
        print '************** POST ' + self.requestline[:100] + '**************'

    def do_OPTIONS(self):
        m = re.match(r'^(?P<Method>\w+)\s+/\?MY_PARAM=(?P<DAT>\S+)\s+.*$', self.requestline)
        if m:
            res = m.groupdict()
            logger.debug('************** (%s) %s **************' % (res['Method'], res['DAT'][:100]))
            fpDigests.append(res['DAT'])
            logger.debug('*** HEADERS: %s ***\n*** HURRAY %d ***' % (repr(self.headers), len(res['DAT'])))
            logger.debug('Got %d bytes' % len(res['DAT']))
            logger.debug('************** %s **************\n' % res['DAT'])
        else:
            pass
        self.send_response(200, "ok")
        logger.debug('************** OPTIONS ' + self.requestline[:100] + '**************')
        self.send_header('Access-Control-Allow-Origin', 'http://www.tnuva.co.il')
        self.send_header('Access-Control-Allow-Methods', 'GET, POST, OPTIONS')
        self.send_header('Access-Control-Allow-Headers', 'X-Requested-With, Content-type, Content-length')



if __name__ == '__main__':
    httpd = SocketServer.TCPServer(("", 80), MyHandler)
    logger.debug("serving at port %d " % 80)
    httpd.serve_forever()
