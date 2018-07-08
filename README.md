# Qiwi TopUp example request
Simple request to Qiwi TopUp (most hated xml api in the world)
  
  
## Balance
```php
        $myCurl = curl_init();
        $headers = array();
        $headers[] = 'Accept: application/xml';
        $headers[] = 'Content-Type: application/xml';
        $request = '<request>
                	    <request-type>ping</request-type>
                	    <terminal-id>YOUR_LOGIN</terminal-id>
                	    <extra name="password">YOUR_PWD</extra>
                    </request>';
            
        curl_setopt_array($myCurl, array(
            CURLOPT_URL => 'https://api.qiwi.com/xml/topup.jsp',
            CURLOPT_RETURNTRANSFER => true,
            CURLOPT_POST => true,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $request
        ));
        $response = curl_exec($myCurl);
        curl_close($myCurl);
        
        $result = new SimpleXMLElement($response);
        $balance = $result->balances->balance;

        return $balance;
```
  
  
  
## Send payment to Qiwi number (without "+", validate it)  
In `payment->to->amount` type summ, in `payment->to->account-number` type beneficiary's phone number  
```php
                $myCurl = curl_init();
                // $rands just generate transaction number, you can do it 1000 and 1 way
                $rands = round((time() + 10 * 5) / 100);
                $headers = array();
                $headers[] = 'Accept: application/xml';
                $headers[] = 'Content-Type: application/xml';
                $request = '<request>
                        	    <request-type>pay</request-type>
                        	    <terminal-id>YOUR_LOGIN</terminal-id>
                        	    <extra name="password">YOUR_PWD</extra>
                        	    <auth>
                        	    	<payment>
                        	    		<transaction-number>'.$rands.'</transaction-number>
                        	    		<from>
                        	    			<ccy>RUB</ccy>
                        	    		</from>
                        	    		<to>
                                                      <amount>1.00</amount>
                                                      <ccy>RUB</ccy>
                                                      <service-id>99</service-id>
                                                      <account-number>70000000000</account-number>
                        	    		</to>
                        	    	</payment>
                        	    </auth>   
                            </request>';

                 curl_setopt_array($myCurl, array(
                 CURLOPT_URL => 'https://api.qiwi.com/xml/topup.jsp',
                     CURLOPT_RETURNTRANSFER => true,
                     CURLOPT_POST => true,
                     CURLOPT_HTTPHEADER => $headers,
                     CURLOPT_POSTFIELDS => $request
                 ));
                 $response = curl_exec($myCurl);
                 curl_close($myCurl);
```
