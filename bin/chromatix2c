#!/usr/bin/env python2

import sys
import struct
import re


if len(sys.argv) < 2:
    print "Usage: chromatix2c.py chromatix_ovXXXX.so"
    sys.exit(1)

inputFilename = sys.argv[1]
f = open(inputFilename, "rb")

ver = re.search(b'\x01\x03\x00\x00\x01\x00\x00\x00', f.read() )
if ver is None:
  print "ERROR: Can't find chromatix_version 0301"
  sys.exit(1)

if ver.start() < 0x100 | ver.start() > 0x4000:
  print "ERROR: Can't find chromatix_version 0301"
  sys.exit(1)

print "chromatix_version 0301 at",hex(ver.start())

p = ver.start()

n = re.search('_common', inputFilename)
if n is None:
  tempFilename = "chromatix_template.h"
else:
  tempFilename = "chromatix_vfe_template.h"

ft = open(tempFilename)
t = ft.read()
ft.close()

cnt = 0
x = 0

while (1):
  n = re.search('`x\w+\+?\w?`', t[x:])
  if n is None:
    break

  x = x + n.start()
  s = n.group(0)
  k = len(s)

  if s[2] == 'a' and s[3] == '`':
    p = p + 3
    p = p & 0xFFFFFC
    t = t[:x] + t[x+k:]
    continue
  
  vt = s[2]
  if vt != 'u' and vt != 'i' and vt != 'f':
    print "ERROR: incorrect record ",s," pos =",x
    sys.exit(1)
  
  vs = int(s[3])
  if vs != 1 and vs != 2 and vs != 4:
    print "ERROR: incorrect record ",s," pos =",x
    sys.exit(1)

  vc = 1
  n = re.search('a(\d+)', s)
  if n is not None:
    vc = int(n.group(0)[1:])
      
  vp = 0
  n = re.search('\+(\d)', s)
  if n is not None:
    vp = int(n.group(0))
    if vp < 0 or vp >= 4:
      print "ERROR: incorrect record ",s," pos =",x
      sys.exit(1)
  
  fmt = ''
  if vt == 'f':
    fmt = 'f'
  else:
    if vt == 'i':
      if vs == 4:
        fmt = 'i'
      if vs == 2:
        fmt = 'h'
      if vs == 1:
        fmt = 'b'
    if vt == 'u':
      if vs == 4:
        fmt = 'I'
      if vs == 2:
        fmt = 'H'
      if vs == 1:
        fmt = 'B'
  
  vk = ''
  for i in range(1,vc+1):
    f.seek(p)
    vv = struct.unpack(fmt, f.read(vs))[0]
    if vt == 'f':
      vv = "%.6ff" % vv
    if vt == 'u':
      vv = "%u" % vv
    if vt == 'i':
      vv = "%i" % vv
    if p == ver.start():
      vv = '0x0301'
    
    p = p + vs
    if i == vc:
      p = p + vp
    
    vk = vk + vv
    if i != vc:
      vk = vk + ', '
  
  #print x," \t ",s," \t ",vt,vs,vc,vp,vk
  #cnt = cnt +1
    
  t = t[:x] + vk + t[x+k:]


f.close()
    
n = re.search('\.\w+', inputFilename)
if n is None:
  outFilename = inputFilename + '.h'  
else:
  outFilename = inputFilename[0:n.start()] + '.h' 

print "Save result to ",outFilename  
ft = open(outFilename, "w")
ft.write(t)
ft.close()

