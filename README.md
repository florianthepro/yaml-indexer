1. add index.yaml to repo with `keys: wert`

2. add on top of php:
```
<?php function yaml_get($key,$default=null,$file=null){$file=$file?:__DIR__.'/index.yaml';if(!is_readable($file))return $default;$lines=preg_split("/\r\n|\n|\r/",(string)file_get_contents($file));foreach($lines as $line){$line=preg_replace('/\s+#.*$/','',rtrim($line));if($line===''||$line==='---'||$line==='...')continue;if(preg_match('/^\s*'.preg_quote((string)$key,'/').'\s*:\s*(.*?)\s*$/',$line,$m)){$v=trim($m[1]);$v=trim($v," \t\n\r\0\x0B\"'");return $v===''?$default:$v;}}return $default;}function f_yaml($key,$file=null){$v=yaml_get($key,null,$file);return $v===null?null:basename((string)$v);} ?>
```
inside php:
```
function yaml_get($key,$default=null,$file=null){$file=$file?:__DIR__.'/index.yaml';if(!is_readable($file))return $default;$lines=preg_split("/\r\n|\n|\r/",(string)file_get_contents($file));foreach($lines as $line){$line=preg_replace('/\s+#.*$/','',rtrim($line));if($line===''||$line==='---'||$line==='...')continue;if(preg_match('/^\s*'.preg_quote((string)$key,'/').'\s*:\s*(.*?)\s*$/',$line,$m)){$v=trim($m[1]);$v=trim($v," \t\n\r\0\x0B\"'");return $v===''?$default:$v;}}return $default;}function f_yaml($key,$file=null){$v=yaml_get($key,null,$file);return $v===null?null:basename((string)$v);}
```

3. use `f_yaml('key')` inside php and `<?=f_yaml('key')?>`(escaped`<?=h(f_yaml('key'))?>`) in html
