#!/usr/bin/python

import sys, time, easysnmp
import math
from math import * #import all
from easysnmp import Session
#to import snmpget and exceptions and snmpwalk from easysnmp simply place a star
from easysnmp import * #import all

#%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#
my_agent_info = sys.argv[1]
all = my_agent_info.split(':')#to split the ip:port:community CL
my_agent_addr = all[0]
my_agent_port = all[1]
my_agent_comm = all[2]
samp_freq = float(sys.argv[2])#sample frequency
nsamp = int(sys.argv[3])#number of samples
time_samp = 1/samp_freq #sample time
oids = []
new_oid = []#listone
old_oid = []#listtwo
#%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#

for i in range(4,len(sys.argv)):## store the oids CL in array and in for loop prefer using variable names as i and j 
	oids.append(sys.argv[i])
oids.insert(0,'1.3.6.1.2.1.1.3.0')##starting oid is SysUpTime oid
#%%%%%%counting oids given in CL%%%%%#

def myfunc():
	global new_oid, present_time
	#%%%%global declaration%%%%%%%%%%# 
	session=Session(hostname=my_agent_addr,remote_port=my_agent_port,community='public',version=2,timeout=1,retries=1) #snmp get -v2c -c -Onvt
	my_response_is = session.get(oids)
	old_oid=[]
	

	for j in range(1,len(my_response_is)):
		if my_response_is[j].value!='NOSUCHOBJECT' and my_response_is[j].value!='NOSUCHINSTANCE':
			old_oid.append(int(my_response_is[j].value))
			
			if count!=0 and len(new_oid)>0:
				my_diff_oid = int(old_oid[j-1]) - int(new_oid[j-1])
				my_diff_time = round(past_time-present_time,1)#round to nearest 1 of decimal or can also use ceil from math
				my_rate = int(my_diff_oid / my_diff_time)
				if my_rate < 0 :
					if my_response_is[j].snmp_type == 'COUNTER32': 
						my_diff_oid = my_diff_oid + 2**32#for 32-bit counter
						print(str(past_time) +"|"+ str(my_diff_oid / my_diff_time) +"|")
					elif my_response_is[j].snmp_type == 'COUNTER64':
						my_diff_oid = my_diff_oid + 2**64#for 64-bit counter
						print(str(past_time) +"|"+ str(my_diff_oid / my_diff_time) +"|")
				else:
					print(str(past_time) +"|"+ str(my_rate) +"|")

	new_oid = old_oid
	present_time = past_time
	#%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%#


if nsamp==-1:
	count=0
	new_oid=[]
	while True:
		past_time = (time.time())
		myfunc()
		time_response = (time.time())
		count = count+1
		time.sleep(abs(time_samp - time_response + past_time))
else:
	new_oid = []
	for count in range(0,nsamp+1):
		past_time = (time.time())
		myfunc()
		time_response = (time.time())
		time.sleep(abs(time_samp - time_response + past_time))
		
