#!/bin/bash
while [ -n "$1" ]
do
case "$1" in
-f) newfile="$2"
 echo "Found the -f option with $newfile"
 shift ;;
-i)state="$2" 
echo "$state" >> $HOME/ishamrai/ponomarev/temp.txt
echo "Found the -i option with $state states test value"
shift ;;
-v) param="$2"
echo "Found the -v option, with parameter value $param"
rm -R $HOME/ishamrai/ponomarev/test/$param
mkdir $HOME/ishamrai/ponomarev/test/$param
mkdir $HOME/ishamrai/ponomarev/temp
cd $HOME/ishamrai/ponomarev/test/$param
for i in 1 2 3 4
do
echo "making node char$i"
export SSHPORT="1173$i"
export P2PPORT="1172$i"
export RPCPORT="1171$i"
export ADMPORT="1170$i"
export NDNAME="Char261620$i"
export H2PORT="1174$i"
mkdir $HOME/ishamrai/ponomarev/test/$param/char$i 
cd $HOME/ishamrai/ponomarev/distr
envsubst  < "node_distr.conf" > $HOME/ishamrai/ponomarev/test/$param/char$i/node.conf
echo "$SSHPORT" > $HOME/ishamrai/ponomarev/temp/sshport$i
echo "$RPCPORT" > $HOME/ishamrai/ponomarev/temp/rpcport$i
echo "$NDNAME" > $HOME/ishamrai/ponomarev/temp/ndname$i
cp $HOME/ishamrai/ponomarev/distr/$param/corda.jar $HOME/ishamrai/ponomarev/test/$param/char$i/
echo "Initial registration of node char$i"
cd $HOME/ishamrai/ponomarev/test/$param/
cd ${PWD}/char$i
$HOME/ishamrai/ponomarev/zulu/bin/java -jar corda.jar initial-registration --network-root-truststore-password password --network-root-truststore $HOME/ishamrai/ponomarev/distr/nightwatchv4-network-root-truststore.jks #>> /dev/null # sta$
 sleep 1
cd $HOME/ishamrai/ponomarev/distr/
cp corda-finance-contracts-4.4-RC06.jar $HOME/ishamrai/ponomarev/test/$param/char$i/cordapps/
cp corda-finance-workflows-4.5-RC04.jar $HOME/ishamrai/ponomarev/test/$param/char$i/cordapps/
cd $HOME/ishamrai/ponomarev/distr/$param
done
cd $HOME/ishamrai/ponomarev/distr/
 file="$HOME/ishamrai/ponomarev/temp.txt"
 for val in $(cat $file)
 do
SSHPORT1=`cat $HOME/ishamrai/ponomarev/temp/sshport1`
SSHPORT2=`cat $HOME/ishamrai/ponomarev/temp/sshport2`
SSHPORT3=`cat $HOME/ishamrai/ponomarev/temp/sshport3`
SSHPORT4=`cat $HOME/ishamrai/ponomarev/temp/sshport4`
RPCPORT1=`cat $HOME/ishamrai/ponomarev/temp/rpcport1`
RPCPORT2=`cat $HOME/ishamrai/ponomarev/temp/rpcport2`
RPCPORT3=`cat $HOME/ishamrai/ponomarev/temp/rpcport3`
RPCPORT4=`cat $HOME/ishamrai/ponomarev/temp/rpcport4`
NDNAME1=`cat $HOME/ishamrai/ponomarev/temp/ndname1`
NDNAME2=`cat $HOME/ishamrai/ponomarev/temp/ndname2`
NDNAME3=`cat $HOME/ishamrai/ponomarev/temp/ndname3`
NDNAME4=`cat $HOME/ishamrai/ponomarev/temp/ndname4`
dir=$HOME/ishamrai/ponomarev/test/$param/
cd "$dir"
 echo "this is $dir"
   echo "now is $val states test begining"
  echo "step1: remove log and bases"
   rm -R ${PWD}/char1/logs/ # remove the log
   rm -R ${PWD}/char2/logs/ # remove the log
   rm -R ${PWD}/char3/logs/ # remove the log
   rm -R ${PWD}/char4/logs/ # remove the log
   rm ${PWD}//char1/persistence.mv.db # remove the base
   rm ${PWD}/char2/persistence.mv.db # remove the base
   rm ${PWD}/char3/persistence.mv.db # remove the base
   rm ${PWD}/char4/persistence.mv.db # remove the base
   rm ${PWD}/char1/persistence.trace.db # remove the base
   rm ${PWD}/char2/persistence.trace.db # remove the base
   rm ${PWD}/char3/persistence.trace.db # remove the base
   rm ${PWD}/char4/persistence.trace.db # remove the base
  echo "step1: starting the node"
   cd ${PWD}/char1 # variable for starting the node
  $HOME/ishamrai/ponomarev/zulu/bin/java -jar corda.jar & >> /dev/null # start in background the node
  echo "step1_1: wait for the ssh open port"
   while ! nc -z localhost $SSHPORT1; do
   sleep 1
   done
   while
   a=$(grep -c 'P2PMessaging' ${PWD}/logs/node-r3-exposed-srv07.log)
   b=0
   [[ "$a" -eq "$b" ]]
   do
   sleep 1
   echo "$a" >> /dev/null
   done
   echo "ssh port is open"
cd $HOME/ishamrai/ponomarev/test/$param/
echo "step2_2: starting node char2"
   cd ${PWD}/char2 # variable for starting the node
   $HOME/ishamrai/ponomarev/zulu/bin/java -jar corda.jar & >> /dev/null # start in background the node
   echo "step2_3: wait for the ssh open port"
   while ! nc -z localhost $SSHPORT2; do   
   sleep 1
   done
   while
   a=$(grep -c 'P2PMessaging' ${PWD}/logs/node-r3-exposed-srv07.log)
   b=0
   [[ "$a" -eq "$b" ]]
   do
   sleep 1
   echo "$a" >> /dev/null
   done
   echo "ssh port is open"
cd $HOME/ishamrai/ponomarev/test/$param/
   echo "step2_4: starting node char3"
   cd ${PWD}/char3 # variable for starting the node
   $HOME/ishamrai/ponomarev/zulu/bin/java -jar corda.jar & >> /dev/null # start in background the node
   echo "step2_5: wait for the ssh open port"
   while ! nc -z localhost $SSHPORT3; do   
   sleep 1
   done
   while
   a=$(grep -c 'P2PMessaging' ${PWD}/logs/node-r3-exposed-srv07.log)
   b=0
   [[ "$a" -eq "$b" ]]
   do
   sleep 1
   echo "$a" >> /dev/null
   done
   echo "ssh port is open"
cd $HOME/ishamrai/ponomarev/test/$param/
   echo "step2_6: starting node char4"
   cd ${PWD}/char4 # variable for starting the node
   $HOME/ishamrai/ponomarev/zulu/bin/java -jar corda.jar & >> /dev/null # start in background the node
   echo "step2_7: wait for the ssh open port"
   while ! nc -z localhost $SSHPORT4; do   
   sleep 1
   done
   while
   a=$(grep -c 'P2PMessaging' ${PWD}/logs/node-r3-exposed-srv07.log)
   b=0
   [[ "$a" -eq "$b" ]]
   do
   sleep 1
   echo "$a" >> /dev/null
   done
   echo "ssh port is open"
   sleep 20
cd $HOME/ishamrai/ponomarev/distr/
   echo "step3: starting the test"
   teststart=$('date')
   echo "Test started at $teststart"
   export BEHAVE_GRAPHITE_FILE=${HOME}/ishamrai/ponomarev/reports/$newfile
   echo "making money on char1 and char2"
   java -Xmx4096m -cp $HOME/ishamrai/ponomarev/rpc-client/rpc-client-ent-v45.jar:$HOME/ishamrai/ponomarev/test/$param/char1/cordapps/corda-finance-workflows-4.5-RC04.jar:$HOME/ishamrai/ponomarev/test/$param/char1/cordapps/corda-finance-contracts-4.4-RC06.jar net.corda.behave.tools.rpcClient.MainKt cash-issue-nft localhost:$RPCPORT1 corda 12345678 --nodename $NDNAME1 --timeout=18000 "1 RUB" "0 RUB" --ntimes=$val --ltimes=1 --delay=0 --duration=0 --notracked=true  & >> /dev/null
   java -Xmx4096m -cp $HOME/ishamrai/ponomarev/rpc-client/rpc-client-ent-v45.jar:$HOME/ishamrai/ponomarev/test/$param/char2/cordapps/corda-finance-workflows-4.5-RC04.jar:$HOME/ishamrai/ponomarev/test/$param/char2/cordapps/corda-finance-contracts-4.4-RC06.jar net.corda.behave.tools.rpcClient.MainKt cash-issue-nft localhost:$RPCPORT2 corda 12345678 --nodename $NDNAME2 --timeout=18000 "1 RUB" "0 RUB" --ntimes=$val --ltimes=1 --delay=0 --duration=0 --notracked=true
   echo "transfer money from Char1 and Char2 and back"
   java -Xmx4096m -cp $HOME/ishamrai/ponomarev/rpc-client/rpc-client-ent-v45.jar:$HOME/ishamrai/ponomarev/test/$param/char1/cordapps/corda-finance-workflows-4.5-RC04.jar:$HOME/ishamrai/ponomarev/test/$param/char1/cordapps/corda-finance-contracts-4.4-RC06.jar net.corda.behave.tools.rpcClient.MainKt cash-payment-nft localhost:$RPCPORT1 corda 12345678 --nodename $NDNAME1 --notary="R3 HoldCo LLC" --timeout=18000 "$val RUB" "0 RUB" "O=$NDNAME2, L=Saratov, C=RU" --ntimes=1 --ltimes=1 --delay=0 --anonymous=false & >> /dev/null
   java -Xmx4096m -cp $HOME/ishamrai/ponomarev/rpc-client/rpc-client-ent-v45.jar:$HOME/ishamrai/ponomarev/test/$param/char2/cordapps/corda-finance-workflows-4.5-RC04.jar:$HOME/ishamrai/ponomarev/test/$param/char2/cordapps/corda-finance-contracts-4.4-RC06.jar net.corda.behave.tools.rpcClient.MainKt cash-payment-nft localhost:$RPCPORT2 corda 12345678 --nodename $NDNAME2 --notary="R3 HoldCo LLC" --timeout=18000 "$val RUB" "0 RUB" "O=$NDNAME1, L=Saratov, C=RU" --ntimes=1 --ltimes=1 --delay=0 --anonymous=false
   echo "transfer money from Char1 to char3"
   java -Xmx4096m -cp $HOME/ishamrai/ponomarev/rpc-client/rpc-client-ent-v45.jar:$HOME/ishamrai/ponomarev/test/$param/char1/cordapps/corda-finance-workflows-4.5-RC04.jar:$HOME/ishamrai/ponomarev/test/$param/char1/cordapps/corda-finance-contracts-4.4-RC06.jar net.corda.behave.tools.rpcClient.MainKt cash-payment-nft localhost:$RPCPORT1 corda 12345678 --nodename $NDNAME1 --notary="R3 HoldCo LLC" --timeout=18000 "89 RUB" "0 RUB" "O=$NDNAME3, L=Saratov, C=RU" --ntimes=1 --ltimes=1 --delay=0 --anonymous=false
   echo "transfer money from Char2 to char4"
   java -Xmx4096m -cp $HOME/ishamrai/ponomarev/rpc-client/rpc-client-ent-v45.jar:$HOME/ishamrai/ponomarev/test/$param/char2/cordapps/corda-finance-workflows-4.5-RC04.jar:$HOME/ishamrai/ponomarev/test/$param/char2/cordapps/corda-finance-contracts-4.4-RC06.jar net.corda.behave.tools.rpcClient.MainKt cash-payment-nft localhost:$RPCPORT2 corda 12345678 --nodename $NDNAME2 --notary="R3 HoldCo LLC" --timeout=18000 "97 RUB" "0 RUB" "O=$NDNAME4, L=Saratov, C=RU" --ntimes=1 --ltimes=1 --delay=0 --anonymous=false
   echo "transfer money from Char3 to char4"
   java -Xmx4096m -cp $HOME/ishamrai/ponomarev/rpc-client/rpc-client-ent-v45.jar:$HOME/ishamrai/ponomarev/test/$param/char3/cordapps/corda-finance-workflows-4.5-RC04.jar:$HOME/ishamrai/ponomarev/test/$param/char3/cordapps/corda-finance-contracts-4.4-RC06.jar net.corda.behave.tools.rpcClient.MainKt cash-payment-nft localhost:$RPCPORT4 corda 12345678 --nodename $NDNAME3 --notary="R3 HoldCo LLC" --timeout=18000 "89 RUB" "0 RUB" "O=$NDNAME4, L=Saratov, C=RU" --ntimes=1 --ltimes=1 --delay=0 --anonymous=false
   testend=$('date')
   echo "Total test duration: ${SECONDS}"
   echo "Test completed at ${testend}"
   echo "step6: shutdown the node char1"
   echo "processes of started nodes"
   a=`cat $HOME/ishamrai/ponomarev/test/$param/char1/process-id`
   b=`cat $HOME/ishamrai/ponomarev/test/$param/char2/process-id`
   c=`cat $HOME/ishamrai/ponomarev/test/$param/char3/process-id`
   d=`cat $HOME/ishamrai/ponomarev/test/$param/char4/process-id`
   echo $a
   echo $b
   echo $c
   echo $d
   kill $a
   kill $b
   kill $c
   kill $d
cd $HOME/ishamrai/ponomarev/distr/
   sleep 3
 done
shift ;;
#-f) newfile="$2"
#  echo "Found the -c option with $newfile";;
--) shift
break ;;
*) echo "$1 is not an option";;
esac
shift
done
count=1
for param in "$@"
do
echo "Parameter #$count: $param"
count=$(( $count + 1 ))
done
rm $HOME/ishamrai/ponomarev/temp.txt
rm -R $HOME/ishamrai/ponomarev/temp
echo "temp files deleted"
