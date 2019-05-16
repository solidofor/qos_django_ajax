# qos_django_ajax
QoS Python Django Ajax Eample of MoMo Api Integration
#Django file

def api(request):
    context = {  
        'status':"info",
        'text':"Not Set",
        'code':"Neutral",
    }
    return render(request, 'index.html',context)
def post(request):
    if request.method == &quot;POST&quot;:
        #keys
		context = 'http://ip_link'
        headers = {'content-type':'application/json'}
        url = context + '/QosicBridge/user/requestpayment'
		raw_url = url;
        msisdn = request.POST['msisdn']
        amount = request.POST['amount']
        firstname = request.POST['firstname']
        lastname = request.POST['lastname']
        clientid = request.POST['clientid']
        transref = (randint(1000000, 9999999))##integer
        transref_str = str(transref) ##to string
        server_pass = 'server_pass'
        server_user = 'server_user'
        url = context +&quot;/QosicBridge/user/requestpayment?msisdn=&quot;+msisdn+&quot;&amp;amount=&quot;+amount+&quot;&amp;firstname=&quot;+firstname+&quot;&amp;lastname=&quot;+lastname+&quot;&amp;transref=&quot;+transref_str+&quot;&amp;ClientID=&quot;+clientid
        data = json.dumps({
            &quot;msisdn&quot;: msisdn,
            &quot;amount&quot;: amount,
            &quot;firstname&quot;:firstname,
            &quot;lastname&quot;:lastname,
            &quot;transref&quot; :transref,
            &quot;clientid&quot;: clientid,
            })
            
        # return HttpResponse(str(url)) ###--##TEST##--##
        
        r = requests.post(url, auth=(server_user, server_pass), headers=headers, data=data,timeout=20)
        # //load the json to a string
        resp = json.loads(r.text)
        
        # return HttpResponse(str(resp['transref']))
        if r.status_code == 200:
                data = r.json() ##response from server
                # return HttpResponse(str(url))
                # return HttpResponse(pprint(data))#console
                # return HttpResponse(print(data)) #console
                context = {  
                    'status':&quot;success&quot;,
                    'text':'Status is OKAY',
                    'code':r.status_code,
                    'responsecode':resp['responsecode'],
                    'responsemsg':resp['responsemsg'],
                    'transref':resp['transref'],
                    'comment':resp['comment']+', '+clientid,
                    'clientid':clientid,
					
					'server_link':raw_url,
					'server_user':server_user,
					'server_pass':server_pass,
                }
                return render(request, 'index.html',context)
        elif r.status_code == 202:
                context = {  
                    'status':&quot;success&quot;,
                    'text':&quot;

Expires in &lt;span id=&quot;time&quot;&gt;05:00&lt;/span&gt;

Check Your Phone; Awaiting Confirmation

&lt;i class=&quot;fa fa-spinner fa-spin&quot; style=&quot;font-size: 60px;&quot;&gt;&lt;/i&gt;&quot;,
                    'code':r.status_code,
                    'responsecode':resp['responsecode'],
                    'responsemsg':resp['responsemsg'],
                    'transref':resp['transref'],
                    'comment':resp['comment'],
                    'clientid':clientid,
                }
                return render(request, 'index.html',context)
        elif r.status_code == 401:
                context = {  
                    'status':&quot;danger&quot;,
                    'text':'Unauthorized',
                    'code':r.status_code,
                    'responsecode':resp['responsecode'],
                    'responsemsg':resp['responsemsg'],
                    'transref':resp['transref'],
                    'comment':resp['comment'],
                }
                return render(request, 'index.html',context)
        else:
                context = {  
                    'status':&quot;danger&quot;,
                    'text':'Failed',
                    'code':r.status_code,
                    'responsecode':resp['responsecode'],
                    'responsemsg':resp['responsemsg'],
                    'transref':resp['transref'],
                    'comment':resp['comment'],
                }
                # return JsonResponse({'error': 'Some error'}, status=400)
                return render(request, 'index.html',context)
    else:
                context = {  
                    'status':&quot;warning&quot;,
                    'text':'Invalid Method;',
                    'code':&quot;-&quot;,
                    'responsecode':'-',
                    'responsemsg':'-',
                    'transref':'-',
                    'comment':'-',
                }
                return render(request, 'index.html',context)

