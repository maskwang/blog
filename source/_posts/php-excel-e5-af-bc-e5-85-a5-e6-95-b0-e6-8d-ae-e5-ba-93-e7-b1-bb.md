---
title: php- EXCEL导入数据库类
id: 547
categories:
  - Php
  - 技术专栏
  - 编程语言
date: 2012-05-11 12:05:36
tags:
---

用法：
首先包含下面的excel_class类
Read_Excel_File($file, $return);
$file：文件名
$return :返回结果

类如下：<!--more-->
<pre lang="php"><!--?php 
define('ABC_CRITICAL',      0);
define('ABC_ERROR',         1);
define('ABC_ALERT',         2);
define('ABC_WARNING',       3);
define('ABC_NOTICE',        4);
define('ABC_INFO',          5);
define('ABC_DEBUG',         6);
define('ABC_TRACE',         7);
define('ABC_VAR_DUMP',      8);
define('ABC_NO_LOG',      -1);
$php_version = split( "\.", phpversion() );
if( $php_version[0] == 4 &#038;& $php_version[1] <= 1 ) {
    if( !function_exists('var_export') ) {
        function var_export( $exp, $ret ) {
ob_start();
var_dump( $exp );
$result = ob_get_contents();
ob_end_clean();
return $result;
}}}function print_bt()
{
print "<code-->\n";
$cs = debug_backtrace();
for( $i = 1; $i &lt; count($cs) ; $i++ )
{
$item = $cs[ $i ];
for( $j = 0; $j &lt; count($item['args']); $j++ )
if( is_string($item['args'][$j]) )
$item['args'][$j] = "\"" . $item['args'][$j] . "\"";
$args = join(",", $item['args'] );
if( isset( $item['class'] ) )
$str = sprintf("%s(%d): %s%s%s(%s)",
$item['file'],
$item['line'],
$item['class'],
$item['type'],
$item['function'],
$args );
else
$str = sprintf("%s(%d): %s(%s)",
$item['file'],
$item['line'],
$item['function'],
$args );
echo $str . "
\n";
}print "\n";
}function _die( $str )
{
print "Script died with reason: $str
\n";
print_bt();
exit();
}class DebugOut
{
var $priorities = array(ABC_CRITICAL    =&gt; 'critical',
                        ABC_ERROR       =&gt; 'error',
                        ABC_ALERT       =&gt; 'alert',
                        ABC_WARNING     =&gt; 'warning',
                        ABC_NOTICE      =&gt; 'notice',
                        ABC_INFO        =&gt; 'info',
                        ABC_DEBUG       =&gt; 'debug',
                        ABC_TRACE       =&gt; 'trace',
                        ABC_VAR_DUMP        =&gt; 'dump'
                        );
var $_ready = false;
var $_currentPriority = ABC_DEBUG;
var $_consumers = array();
var  $_filename;
var  $_fp;
var  $_logger_name;

 function DebugOut($name, $logger_name, $level ){
     $this-&gt;_filename = $name;
     $this-&gt;_currentPriority = $level;
     $this-&gt;_logger_name = $logger_name;
     if ($level &gt; ABC_NO_LOG){
        $this-&gt;_openfile();
     }     /*Destructor Registering*/
     register_shutdown_function(array($this,"close"));
 } function log($message, $priority = ABC_INFO) {
        // Abort early if the priority is above the maximum logging level.
        if ($priority &gt; $this-&gt;_currentPriority) {
            return false;
        }        // Add to loglines array
        return $this-&gt;_writeLine($message, $priority, strftime('%b %d %H:%M:%S'));
 } function dump($variable,$name) {
       $priority = ABC_VAR_DUMP;
       if ($priority &gt; $this-&gt;_currentPriority ) {
            return false;
       }       $time = strftime('%b %d %H:%M:%S');
       $message = var_export($variable,true);
       return fwrite($this-&gt;_fp,
                     sprintf("%s %s [%s] variable %s = %s \r\n",
                             $time,
                             $this-&gt;_logger_name,
                             $this-&gt;priorities[$priority],
                             $name,
                             $message)
                             );
 } function info($message) {
        return $this-&gt;log($message, ABC_INFO);
 } function debug($message) {
        return $this-&gt;log($message, ABC_DEBUG);
 } function notice($message) {
        return $this-&gt;log($message, ABC_NOTICE);
 } function warning($message) {
        return $this-&gt;log($message, ABC_WARNING);
 } function trace($message) {
        return $this-&gt;log($message, ABC_TRACE);
 } function error($message) {
        return $this-&gt;log($message, ABC_ERROR);
 } /**
  * Writes a line to the logfile
  *
  * @param  string $line      The line to write
  * @param  integer $priority The priority of this line/msg
  * @return integer           Number of bytes written or -1 on error
  * @access private
  */
 function _writeLine($message, $priority, $time) {
    if( fwrite($this-&gt;_fp, sprintf("%s %s [%s] %s\r\n", $time, $this-&gt;_logger_name, $this-&gt;priorities[$priority], $message)) ) {
        return fflush($this-&gt;_fp);
    } else {
        return false;
    } } function _openfile() {
    if (($this-&gt;_fp = @fopen($this-&gt;_filename, 'a')) == false) {
        return false;
    }        return true;
 } function close(){
    if($this-&gt;_currentPriority != ABC_NO_LOG){
        $this-&gt;info("Logger stoped");
        return fclose($this-&gt;_fp);
    } } /*
  * Managerial Functions.
  *
  */
 function Factory($name, $logger_name, $level) {
    $instance = new DebugOut($name, $logger_name, $level);
    return $instance;
 } function &amp;getWriterSingleton($name, $logger_name, $level = ABC_DEBUG){
      static $instances;
      if (!isset($instances)){
        $instances = array();
      }      $signature = serialize(array($name, $level));
      if (!isset($instances[$signature])) {
            $instances[$signature] = DebugOut::Factory($name, $logger_name, $level);
      }      
      return $instances[$signature];
 } function attach(&amp;$logObserver) {
    if (!is_object($logObserver)) {
        return false;
    }    $logObserver-&gt;_listenerID = uniqid(rand());
    $this-&gt;_listeners[$logObserver-&gt;_listenerID] = &amp;$logObserver;
 }}define ('ABC_BAD_DATE', -1);
class ExcelDateUtil{

/*
 * return 1900 Date as integer TIMESTAMP.
 * for UNIX date must be
 *
 */
function xls2tstamp($date) {
$date=$date&gt;25568?$date:25569;
/*There was a bug if Converting date before 1-1-1970 (tstamp 0)*/
   $ofs=(70 * 365 + 17+2) * 86400;
   return ($date * 86400) - $ofs;
}function getDateArray($xls_date){
    $ret = array();
    // leap year bug
    if ($xls_date == 60) {
        $ret['day']   = 29;
        $ret['month'] = 2;
        $ret['year']  = 1900;
        return $ret;
    } else if ($xls_date &lt; 60) {
        // 29-02-1900 bug
        $xls_date++;
    }    // Modified Julian to DMY calculation with an addition of 2415019
    $l = $xls_date + 68569 + 2415019;
    $n = (int)(( 4 * $l ) / 146097);
    $l = $l - (int)(( 146097 * $n + 3 ) / 4);
    $i = (int)(( 4000 * ( $l + 1 ) ) / 1461001);
    $l = $l - (int)(( 1461 * $i ) / 4) + 31;
    $j = (int)(( 80 * $l ) / 2447);
    $ret['day'] = $l - (int)(( 2447 * $j ) / 80);
    $l = (int)($j / 11);
    $ret['month'] = $j + 2 - ( 12 * $l );
    $ret['year'] = 100 * ( $n - 49 ) + $i + $l;
    return $ret;
}function isInternalDateFormat($format) {
    $retval =false;
    switch(format) {
    // Internal Date Formats as described on page 427 in
    // Microsoft Excel Dev's Kit...
        case 0x0e:
        case 0x0f:
        case 0x10:
        case 0x11:
        case 0x12:
        case 0x13:
        case 0x14:
        case 0x15:
        case 0x16:
        case 0x2d:
        case 0x2e:
        case 0x2f:
        // Additional internal date formats found by inspection
        // Using Excel v.X 10.1.0 (Mac)
        case 0xa4:
        case 0xa5:
        case 0xa6:
        case 0xa7:
        case 0xa8:
        case 0xa9:
        case 0xaa:
        case 0xab:
        case 0xac:
        case 0xad:
        $retval = true; break;
        default: $retval = false; break;
    }         return $retval;
}}define('EXCEL_FONT_RID',0x31);
define('XF_SCRIPT_NONE',0);
define('XF_SCRIPT_SUPERSCRIPT',1);
define('XF_SCRIPT_SUBSCRIPT',2);
define('XF_UNDERLINE_NONE',0x0);
define('XF_UNDERLINE_SINGLE',0x1);
define('XF_UNDERLINE_DOUBLE',0x2);
define('XF_UNDERLINE_SINGLE_ACCOUNTING',0x3);
define('XF_UNDERLINE_DOUBLE_ACCOUNTING',0x4);
define('XF_STYLE_ITALIC', 0x2);
define('XF_STYLE_STRIKEOUT', 0x8);
define('XF_BOLDNESS_REGULAR',0x190);
define('XF_BOLDNESS_BOLD',0x2BC);

class ExcelFont {

 function basicFontRecord() {
    return  array('size'     =&gt; 10,
                    'script'   =&gt; XF_SCRIPT_NONE,
                    'undeline' =&gt; XF_UNDERLINE_NONE,
                    'italic'   =&gt; false,
                    'strikeout'=&gt; false,
                    'bold'     =&gt; false,
                    'boldness' =&gt; XF_BOLDNESS_REGULAR,
                    'palete'   =&gt; 0,
                    'name'     =&gt; 'Arial');
 } function getFontRecord(&amp;$wb,$ptr) {
    $retval = array('size'     =&gt; 0,
                    'script'   =&gt; XF_SCRIPT_NONE,
                    'undeline' =&gt; XF_UNDERLINE_NONE,
                    'italic'   =&gt; false,
                    'strikeout'=&gt; false,
                    'bold'     =&gt; false,
                    'boldness' =&gt; XF_BOLDNESS_REGULAR,
                    'palete'   =&gt; 0,
                    'name'     =&gt; '');
    $retval['size'] = (ord($wb[$ptr])+ 256*ord($wb[$ptr+1]))/20;
    $style=ord($wb[$ptr+2]);
    if (($style &amp; XF_STYLE_ITALIC) != 0) {
        $retval['italic'] = true;
    }    if (($style &amp; XF_STYLE_STRIKEOUT) != 0) {
        $retval['strikeout'] = true;
    }    $retval['palete'] = ord($wb[$ptr+4])+256*ord($wb[$ptr+5]);
    $retval['boldness'] = ord($wb[$ptr+6])+256*ord($wb[$ptr+7]);
    $retval['bold'] = $retval['boldness'] == XF_BOLDNESS_REGULAR ? false:true;
    $retval['script'] =  ord($wb[$ptr+8])+256*ord($wb[$ptr+9]);
    $retval['underline'] = ord($wb[$ptr+10]);
    $length = ord($wb[$ptr+14]);
    if($length &gt;0) {
        if(ord($wb[$ptr+15]) == 0) { // Compressed Unicode
            $retval['name'] = substr($wb,$ptr+16,$length);
        } else { // Uncompressed Unicode
            $retval['name'] = ExcelFont::getUnicodeString($wb,$ptr+15,$length);
        }    }    return $retval;
 } function toString(&amp;$record,$index) {
    $retval = sprintf("Font Index = %d \nFont Size =%d\nItalic = %s\nStrikeoout=%s\nPalete=%s\nBoldness = %s Bold=%s\n Script = %d\n Underline = %d\n FontName=%s</pre>

* * *

<pre lang="php">",
                $index,
                $record['size'],
                $record['italic']    == true?"true":"false",
                $record['strikeout'] == true?"true":"false",
                $record['palete'],
                $record['boldness'],
                $record['bold'] == true?"true":"false",
                $record['script'],
                $record['underline'],
                $record['name']
                );
    return $retval;
 } function getUnicodeString(&amp;$string,$offset,$length) {
        $bstring = "";
        $index   = $offset + 1;   // start with low bits.
        for ($k = 0; $k &lt; $length; $k++)
        {
            $bstring = $bstring.$string[$index];
            $index        += 2;
        }        return substr($bstring,0,$length);
 } function ExcelToCSS($rec, $app_font=true, $app_size=true, $app_italic=true, $app_bold=true){
    $ret = "";
    if($app_font==true){
        $ret = $ret."font-family:".$rec['name']."; ";
    }    if($app_size==true){
        $ret = $ret."font-size:".$rec['size']."pt; ";
    }    if($app_bold==true){
        if($rec['bold']==true){
            $ret = $ret."font-weight:bold; ";
        } else {
            $ret = $ret."font-weight:normal; ";
        }    }    if($app_italic==true){
        if($rec['italic']==true){
            $ret = $ret."font-style:italic; ";
        } else {
            $ret = $ret."font-style:normal; ";
        }    }    return $ret;
 }}define ( DP_EMPTY, 0 );
define ( DP_STRING_SOURCE, 1 );
define ( DP_FILE_SOURCE, 2 );
//------------------------------------------------------------------------
class ExcelParserUtil
{
function str2long($str) {
return ord($str[0]) + 256*(ord($str[1]) +
256*(ord($str[2]) + 256*(ord($str[3])) ));
}}//------------------------------------------------------------------------
class DataProvider
{
function DataProvider( $data, $dataType )
{
switch( $dataType )
{
case DP_FILE_SOURCE:
if( !( $this-&gt;_data = @fopen( $data, "rb" )) )
return;
$this-&gt;_size = @filesize( $data );
if( !$this-&gt;_size )
_die("Failed to determine file size.");
break;
case DP_STRING_SOURCE:
$this-&gt;_data = $data;
$this-&gt;_size = strlen( $data );
break;
default:
_die("Invalid data type provided.");
}$this-&gt;_type = $dataType;
register_shutdown_function( array( $this, "close") );
}function get( $offset, $length )
{
if( !$this-&gt;isValid() )
_die("Data provider is empty.");
if( $this-&gt;_baseOfs + $offset + $length &gt; $this-&gt;_size )
_die("Invalid offset/length.");
switch( $this-&gt;_type )
{
case DP_FILE_SOURCE:
{
if( @fseek( $this-&gt;_data, $this-&gt;_baseOfs + $offset, SEEK_SET ) == -1 )
_die("Failed to seek file position specified by offest.");
return @fread( $this-&gt;_data, $length );
}case DP_STRING_SOURCE:
{
$rc = substr( $this-&gt;_data, $this-&gt;_baseOfs + $offset, $length );
return $rc;
}default:
_die("Invalid data type or class was not initialized.");
}}function getByte( $offset )
{
return $this-&gt;get( $offset, 1 );
}function getOrd( $offset )
{
return ord( $this-&gt;getByte( $offset ) );
}function getLong( $offset )
{
$str = $this-&gt;get( $offset, 4 );
return ExcelParserUtil::str2long( $str );
}function getSize()
{
if( !$this-&gt;isValid() )
_die("Data provider is empty.");
return $this-&gt;_size;
}function getBlocks()
{
if( !$this-&gt;isValid() )
_die("Data provider is empty.");
return (int)(($this-&gt;_size - 1) / 0x200) - 1;
}function ReadFromFat( $chain, $gran = 0x200 )
{
$rc = '';
for( $i = 0; $i &lt; count($chain); $i++ )
$rc .= $this-&gt;get( $chain[$i] * $gran, $gran );
return $rc;
}function close()
{
switch($this-&gt;_type )
{
case DP_FILE_SOURCE:
@fclose( $this-&gt;_data );
case DP_STRING_SOURCE:
$this-&gt;_data = null;
default:
$_type = DP_EMPTY;
break;
}}function isValid()
{
return $this-&gt;_type != DP_EMPTY;
}var $_type = DP_EMPTY;
var $_data = null;
var $_size = -1;
var $_baseOfs = 0;
}class ExcelFileParser {
var $dp = null;
var $max_blocks;
var $max_sblocks;
// Internal variables
var $fat;
var $sfat;
// Removed: var $sbd;
// Removed: var $syear;
var $formats;
var $xf;
var $fonts;
    var $dbglog;

    function ExcelFileParser($logfile="",$level=ABC_NO_LOG) {
$this-&gt;dbglog = &amp;DebugOut::getWriterSingleton($logfile,"",$level);
        $this-&gt;dbglog-&gt;info("Logger started");
    }function populateFormat() {
$this-&gt;dbglog-&gt;trace(" populateFormat() function call");
$ret = array (
        0=&gt; "General",
        1=&gt; "0",
        2=&gt; "0.00",
        3=&gt; "#,##0",
        4=&gt; "#,##0.00",
        5=&gt; "($#,##0_);($#,##0)",
        6=&gt; "($#,##0_);[Red]($#,##0)",
        7=&gt; "($#,##0.00);($#,##0.00)",
        8=&gt; "($#,##0.00_);[Red]($#,##0.00)",
        9=&gt; "0%",
        0xa=&gt; "0.00%",
        0xb=&gt; "0.00E+00",
        0xc=&gt; "# ?/?",
        0xd=&gt; "# ??/??",
        0xe=&gt; "m/d/yy",
        0xf=&gt; "d-mmm-yy",
        0x10=&gt; "d-mmm",
        0x11=&gt; "mmm-yy",
        0x12=&gt; "h:mm AM/PM",
        0x13=&gt; "h:mm:ss AM/PM",
        0x14=&gt; "h:mm",
        0x15=&gt; "h:mm:ss",
        0x16=&gt; "m/d/yy h:mm",
        // 0x17 - 0x24 reserved for international and undocumented
        0x17=&gt; "0x17",
        0x18=&gt; "0x18",
        0x19=&gt; "0x19",
        0x1a=&gt; "0x1a",
        0x1b=&gt; "0x1b",
        0x1c=&gt; "0x1c",
        0x1d=&gt; "0x1d",
        0x1e=&gt; "0x1e",
        0x1f=&gt; "0x1f",
        0x20=&gt; "0x20",
        0x21=&gt; "0x21",
        0x22=&gt; "0x22",
        0x23=&gt; "0x23",
        0x24=&gt; "0x24",
        // 0x17 - 0x24 reserved for international and undocumented
        0x25=&gt; "(#,##0_);(#,##0)",
        0x26=&gt; "(#,##0_);[Red](#,##0)",
        0x27=&gt; "(#,##0.00_);(#,##0.00)",
        0x28=&gt; "(#,##0.00_);[Red](#,##0.00)",
        0x29=&gt; "_(*#,##0_);_(*(#,##0);_(* \"-\"_);_(@_)",
        0x2a=&gt; "_($*#,##0_);_($*(#,##0);_($* \"-\"_);_(@_)",
        0x2b=&gt; "_(*#,##0.00_);_(*(#,##0.00);_(*\"-\"??_);_(@_)",
        0x2c=&gt; "_($*#,##0.00_);_($*(#,##0.00);_($*\"-\"??_);_(@_)",
        0x2d=&gt; "mm:ss",
        0x2e=&gt; "[h]:mm:ss",
        0x2f=&gt; "mm:ss.0",
        0x30=&gt; "##0.0E+0",
        0x31=&gt; "@");
            $this-&gt;dbglog-&gt;dump($ret,"\$ret");
            $this-&gt;dbglog-&gt;trace("populateFormat() function return");
        return $ret;
}function xls2tstamp($date) {
$date=$date&gt;25568?$date:25569;
/*There was a bug if Converting date before 1-1-1970 (tstamp 0)*/
   $ofs=(70 * 365 + 17+2) * 86400;
   return ($date * 86400) - $ofs;
}function getDateArray($date) {
   return ExcelDateUtil::getDateArray($date);
}function isDateFormat($val){
$f_i=$this-&gt;xf['format'][$val];
if(preg_match("/[m|d|y]/i",$this-&gt;format[$f_i])!=0){
    if(strrpos($this-&gt;format[$f_i],'[')!=FALSE) {
        $tmp = preg_replace("/(\[\/?)(\w+)([^\]]*\])/","'\\1'.''.'\\3'",$this-&gt;format[$f_i]);
    if(preg_match("/[m|d|y]/i",$tmp)!=0)
       return TRUE;
     else
       return FALSE;
    } else {
        return TRUE;
    }} else
  return FALSE;
}function getUnicodeString($str,$ofs){
   $size=0;
   $i_ofs=0;
/*   if (ord($str[$ofs])==255) {
     $size=ord($str[$ofs])+ 256*(ord($str[$ofs+1]));
     $i_ofs=2;
   } else {*/
     $size=ord($str[$ofs]);
     $i_ofs=1;
/*   }*/
   return substr($str,$ofs+$i_ofs+1,$size);
}function getByteString($str,$ofs){
   $size=0;
   $i_ofs=0;
//   if (ord($str[$ofs])==255) {
//     $size=ord($str[$ofs])+ 256*(ord($str[$ofs+1]));
//     $i_ofs=2;
//   } else {
     $size=ord($str[$ofs]);
     $i_ofs=1;
//   }   return substr($str,$ofs+$i_ofs+1,$size);
}/*
 * Get blocks chain
 */
function get_blocks_chain($start,$small_fat=false) {
$this-&gt;dbglog-&gt;trace("get_blocks_chain(".var_export($start,true).",".var_export($small_fat,true).") function call ");
$chain = array();
$next_block = $start;
if( !$small_fat ) {
while(  ($next_block!=0xfffffffe) &amp;&amp;
($next_block max_blocks) &amp;&amp;
($next_block &lt; count($this-&gt;fat)) )
{
$chain[] = $next_block;
$next_block = $this-&gt;fat[$next_block];
}} else {
while(  ($next_block!=0xfffffffe) &amp;&amp;
($next_block max_sblocks) &amp;&amp;
($next_block &lt; count($this-&gt;sfat)) )
{
$chain[] = $next_block;
$next_block = $this-&gt;sfat[$next_block];
}}if( $next_block != 0xfffffffe )
return false;
$this-&gt;dbglog-&gt;dump($chain,"\$chain");
$this-&gt;dbglog-&gt;trace("get_blocks_chain() function return");
return $chain;
}/* Find stream by name
 *
 */
function find_stream( $dir, $item_name,$item_num=0) {
        $this-&gt;dbglog-&gt;trace("find_stream(".var_export($dir,true).",".var_export($item_name,true).",".var_export($item_num,true).") function call ");
$dt = $dir-&gt;getOrd( $item_num * 0x80 + 0x42 );
$prev = $dir-&gt;getLong( $item_num * 0x80 + 0x44 );
$next = $dir-&gt;getLong( $item_num * 0x80 + 0x48 );
$dir_ = $dir-&gt;getLong( $item_num * 0x80 + 0x4c );
$curr_name = '';
if( ($dt==2) || ($dt==5) )
for( $i=0;
 $i &lt; ( $dir-&gt;getOrd( $item_num * 0x80 + 0x40 ) +
  256 * $dir-&gt;getOrd( $item_num * 0x80 + 0x41 ) )/2-1;
 $i++ )
$curr_name .= $dir-&gt;getByte( $item_num * 0x80 + $i * 2 );
if( (($dt==2) || ($dt==5)) &amp;&amp; (strcmp($curr_name,$item_name)==0) ){
    $this-&gt;dbglog-&gt;trace("find_stream() function return with ".var_export($item_num,true));
return $item_num;
}if( $prev != 0xffffffff ) {
$i = $this-&gt;find_stream( $dir, $item_name, $prev);
if( $i&gt;=0 ){
    $this-&gt;dbglog-&gt;trace("find_stream() function return with ".var_export($i,true));
    return $i;
    }}if( $next != 0xffffffff ) {
$i = $this-&gt;find_stream( $dir, $item_name, $next);
if( $i&gt;=0 ){
    $this-&gt;dbglog-&gt;trace("find_stream() function return with ".var_export($i,true));
    return $i;
}}if( $dir_ != 0xffffffff ) {
$i = $this-&gt;find_stream( $dir, $item_name, $dir_ );
if( $i&gt;=0 ) {
    $this-&gt;dbglog-&gt;trace("find_stream() function return with ".var_export($i,true));
    return $i;
}}        $this-&gt;dbglog-&gt;trace("find_stream() function return with -1");
return -1;
}function rk_decode($rk) {
    $this-&gt;dbglog-&gt;trace("rk_decode(".var_export($rk,true).") function call");
$res = array();
if( $rk &amp; 2 ) {
// integer
$val = ($rk &amp; 0xfffffffc) &gt;&gt; 2;
if( $rk &amp; 1 ) $val = $val / 100;
if (((float)$val) == floor((float)$val)){
   $res['val'] = (int)$val;
   $res['type'] = 1;
} else {
   $res['val'] = (float)$val;
   $res['type'] = 2;
}} else {
// float
$res['type'] = 2;
$frk = $rk;
$fexp =  (($frk &amp; 0x7ff00000) &gt;&gt; 20) - 1023;
$val = 1+(($frk &amp; 0x000fffff) &gt;&gt; 2)/262144;
if( $fexp &gt; 0 ) {
for( $i=0; $idbglog-&gt;trace("rk_decode() function returns");
return $res;
}// Parse worksheet
//-----------------
function parse_worksheet($ws) {
        $this-&gt;dbglog-&gt;debug("parse_worksheet(DATA) function");
if( strlen($ws) dbglog-&gt;trace("parse_worksheet() function returns 7 (Data not Found)");
        return 7;
    }if( strlen($ws) &lt;  4 ){
    $this-&gt;dbglog-&gt;trace("parse_worksheet() function returns 6 (File Corrupted)");
    return 6;
}// parse workbook header
if( strlen($ws) &lt; 256*ord($ws[3])+ord($ws[2]) ) return 6;
if( ord($ws[0]) != 0x09 ) return 6;
$vers = ord($ws[1]);
if( ($vers!=0) &amp;&amp; ($vers!=2) &amp;&amp; ($vers!=4) &amp;&amp; ($vers!=8) )
return 8;
if( $vers!=8 ) {
 $biff_ver = ($ver+4)/2;
} else {
 if( strlen($ws) &lt; 12 ) return 6;
 switch( ord($ws[4])+256*ord($ws[5]) ) {
case 0x0500:
if( ord($ws[0x0a])+256*ord($ws[0x0b]) &lt; 1994 ) {
$biff_ver = 5;
} else {
switch(ord( $ws[8])+256*ord($ws[9]) ) {
 case 2412:
 case 3218:
 case 3321:
/*dbg*/             $this-&gt;dbglog-&gt;debug("Parsed BIFF version is 5");
$biff_ver = 5;
 break;
 default:
    $this-&gt;dbglog-&gt;debug("Parsed BIFF version is 7");
$biff_ver = 7;
break;
}}break;
case 0x0600:
/*DBG*/    $this-&gt;dbglog-&gt;debug("Parsed BIFF version is 8");
$biff_ver = 8;
break;
default:
return 8;
 }}if( $biff_ver &lt; 5 ) {
/*DBG*/  $this-&gt;dbglog-&gt;debug("parse_worksheet() function found ($biff_ver &lt; 5) return 8");
  return 8;
}$ptr = 0;
$data = array('biff_version' =&gt; $biff_ver );
while( (ord($ws[$ptr])!=0x0a) &amp;&amp; ($ptrdbglog-&gt;trace("found NUMBER");
if( ($biff_ver &lt; 3) ){
/*DBG*/         $this-&gt;dbglog-&gt;trace("$biff_ver &lt; 3 break;");
    break;
}if( (ord($ws[$ptr+2])+256*ord($ws[$ptr+3])) &lt; 14 ){
/*DBG*/         $this-&gt;dbglog-&gt;debug("parse_worksheet() return 6");
return 6;
}$row = ord($ws[$ptr+4])+256*ord($ws[$ptr+5]);
$col = ord($ws[$ptr+6])+256*ord($ws[$ptr+7]);
$num_lo = ExcelParserUtil::str2long(substr($ws,$ptr+10,4));
$num_hi = ExcelParserUtil::str2long(substr($ws,$ptr+14,4));
$xf_i = ord($ws[$ptr+8])+256*ord($ws[$ptr+9]);
if($this-&gt;isDateFormat($xf_i)){
$data['cell'][$row][$col]['type'] = 3;
} else {
$data['cell'][$row][$col]['type'] = 2;
}$fonti = $this-&gt;xf['font'][$xf_i];
    $data['cell'][$row][$fc+$i]['font'] = $fonti;

$fexp = (($num_hi &amp; 0x7ff00000) &gt;&gt; 20) - 1023;
$val = 1+(($num_hi &amp; 0x000fffff)+$num_lo/4294967296)/1048576;
if( $fexp &gt; 0 ) {
for( $i=0; $idbglog-&gt;trace("found RK");
if( ($biff_ver &lt; 3) ) break;
if( (ord($ws[$ptr+2])+256*ord($ws[$ptr+3])) &lt; 0x0a )
return 6;
$row  = ord($ws[$ptr+4])+256*ord($ws[$ptr+5]);
$col  = ord($ws[$ptr+6])+256*ord($ws[$ptr+7]);
$xf_i = ord($ws[$ptr+8])+256*ord($ws[$ptr+9]);
$val  = $this-&gt;rk_decode(
ExcelParserUtil::str2long(substr($ws,$ptr+10,4))
);
if($this-&gt;isDateFormat($xf_i)==TRUE){
$data['cell'][$row][$col]['type'] = 3;
} else {
$data['cell'][$row][$col]['type'] = $val['type'];
}$fonti = $this-&gt;xf['font'][$xf_i];
    $data['cell'][$row][$col]['font'] = $fonti;
$data['cell'][$row][$col]['data'] = $val['val'];

if( !isset($data['max_row']) ||
    ($data['max_row'] &lt; $row) )
$data['max_row'] = $row;
if( !isset($data['max_col']) ||
    ($data['max_col'] &lt; $col) )
$data['max_col'] = $col;
break;
  // MULRK
  case 0x00bd:
/*DBG*/  $this-&gt;dbglog-&gt;trace("found  MULL RK");
if( ($biff_ver &lt; 5) ) break;
$sz = ord($ws[$ptr+2])+256*ord($ws[$ptr+3]);
if( $sz &lt; 6 ) return 6;
$row = ord($ws[$ptr+4])+256*ord($ws[$ptr+5]);
$fc = ord($ws[$ptr+6])+256*ord($ws[$ptr+7]);
$lc = ord($ws[$ptr+$sz+2])+256*ord($ws[$ptr+$sz+3]);
for( $i=0; $irk_decode(
ExcelParserUtil::str2long(substr($ws,$ptr+10+$i*6,4))
);
   $xf_i=ord($ws[$ptr+8+$i*6])+256*ord($ws[($ptr+9+$i*6)]);
   if($this-&gt;isDateFormat($xf_i)==TRUE) {
   $data['cell'][$row][$fc+$i]['type'] = 3;
   } else {
   $data['cell'][$row][$fc+$i]['type'] = $val['type'];
   }   $fonti = $this-&gt;xf['font'][$xf_i];
       $data['cell'][$row][$fc+$i]['font'] = $fonti;
   $data['cell'][$row][$fc+$i]['data'] = $val['val'];
}if( !isset($data['max_row']) ||
    ($data['max_row'] &lt; $row) )
$data['max_row'] = $row;
if( !isset($data['max_col']) ||
    ($data['max_col'] &lt; $lc) )
$data['max_col'] = $lc;
break;
  // LABEL
  case 0x0204:
/*DBG*/  $this-&gt;dbglog-&gt;trace("found LABEL");
if( ($biff_ver &lt; 3) ){
    break;
}if( (ord($ws[$ptr+2])+256*ord($ws[$ptr+3])) &lt; 8 ){
return 6;
}$row = ord($ws[$ptr+4])+256*ord($ws[$ptr+5]);
$col = ord($ws[$ptr+6])+256*ord($ws[$ptr+7]);
$xf = ord($ws[$ptr+8])+256*ord($ws[$ptr+9]);
$fonti = $this-&gt;xf['font'][$xf];
$font =  $this-&gt;fonts[$fonti];

$str_len = ord($ws[$ptr+10])+256*ord($ws[$ptr+11]);
if( $ptr+12+$str_len &gt; strlen($ws) )
return 6;
$this-&gt;sst['unicode'][] = false;
$this-&gt;sst['data'][] = substr($ws,$ptr+12,$str_len);
$data['cell'][$row][$col]['type'] = 0;
$sst_ind = count($this-&gt;sst['data'])-1;
$data['cell'][$row][$col]['data'] = $sst_ind;
$data['cell'][$row][$col]['font'] = $fonti;
/*echo str_replace("\n","
\n", ExcelFont::toString($font,$fonti));
    echo "FontRecord for sting ".$this-&gt;sst['data'][$sst_ind]."
";*/
if( !isset($data['max_row']) ||
    ($data['max_row'] &lt; $row) )
$data['max_row'] = $row;
if( !isset($data['max_col']) ||
    ($data['max_col'] &lt; $col) )
$data['max_col'] = $col;

break;
  // LABELSST
  case 0x00fd:
if( $biff_ver &lt; 8 ) break;
if( (ord($ws[$ptr+2])+256*ord($ws[$ptr+3])) &lt; 0x0a )
return 6;
$row = ord($ws[$ptr+4])+256*ord($ws[$ptr+5]);
$col = ord($ws[$ptr+6])+256*ord($ws[$ptr+7]);
$xf = ord($ws[$ptr+8])+256*ord($ws[$ptr+9]);
$fonti = $this-&gt;xf['font'][$xf];
$font = &amp;$this-&gt;fonts[$fonti];
$data['cell'][$row][$col]['type'] = 0;
$sst_ind = ExcelParserUtil::str2long(substr($ws,$ptr+10,4));
$data['cell'][$row][$col]['data'] = $sst_ind;
$data['cell'][$row][$col]['font'] = $fonti;
/*            echo "FontRecord for sting at $row,$col
";
echo str_replace("\n","
\n", ExcelFont::toString($font,$fonti));*/
if( !isset($data['max_row']) ||
    ($data['max_row'] &lt; $row) )
$data['max_row'] = $row;
if( !isset($data['max_col']) ||
    ($data['max_col'] &lt; $col) )
$data['max_col'] = $col;
break;
  // unknown, unsupported or unused opcode
  default:
break;
 } $ptr += 4+256*ord($ws[$ptr+3])+ord($ws[$ptr+2]);
}/*DEBUG*/ $this-&gt;dbglog-&gt;debug("parse_worksheet() function returns ".var_export($data,true)); /*DEBUG*/
return $data;
}// Parse workbook
//----------------
function parse_workbook( $f_header, $dp ) {
/*DBG*/ $this-&gt;dbglog-&gt;debug("parse_workbook() function");
$root_entry_block = $f_header-&gt;getLong(0x30);
$num_fat_blocks = $f_header-&gt;getLong(0x2c);
/*TRC*/ $this-&gt;dbglog-&gt;trace("Header parsed");
$this-&gt;fat = array();
for( $i = 0; $i &lt; $num_fat_blocks; $i++ ){
/*TRC*/$this-&gt;dbglog-&gt;trace("FOR LOOP iteration i =".$i);
$fat_block = $f_header-&gt;getLong( 0x4c + 4 * $i );
$fatbuf = $dp-&gt;get( $fat_block * 0x200, 0x200 );
$fat = new DataProvider( $fatbuf, DP_STRING_SOURCE );
if( $fat-&gt;getSize() &lt; 0x200 ){
/*DBG*/    $this-&gt;dbglog-&gt;debug("parse_workbook() function found (strlen($fat) &lt; 0x200) returns 6");
return 6;
}for( $j=0; $jfat[] = $fat-&gt;getLong( $j * 4 );
$fat-&gt;close();
unset( $fat_block, $fatbuf, $fat );
}/*DBG*/ $this-&gt;dbglog-&gt;dump( $this-&gt;fat, "\$fat" );
if( count($this-&gt;fat) &lt; $num_fat_blocks ) {
/*DBG*/    $this-&gt;dbglog-&gt;debug("parse_workbook() function found (count($this-&gt;fat) &lt; $num_fat_blocks) returns 6");
return 6;
}$chain = $this-&gt;get_blocks_chain($root_entry_block);
$dir = new DataProvider( $dp-&gt;ReadFromFat( $chain ), DP_STRING_SOURCE );
unset( $chain );
$this-&gt;sfat = array();
$small_block = $f_header-&gt;getLong( 0x3c );
if( $small_block != 0xfeffffff ) {
$root_entry_index = $this-&gt;find_stream( $dir, 'Root Entry');
if( $root_entry_index &lt; 0 ) {
/*DBG*/    $this-&gt;dbglog-&gt;debug("parse_workbook() function dont found Root Entry returns 6");
    return 6;
 } 
 $sdc_start_block = $dir-&gt;getLong( $root_entry_index * 0x80 + 0x74 );
 $small_data_chain = $this-&gt;get_blocks_chain($sdc_start_block);
 $this-&gt;max_sblocks = count($small_data_chain) * 8;

 $schain = $this-&gt;get_blocks_chain($small_block); 
 for( $i = 0; $i &lt; count( $schain ); $i++ ) {

$sfatbuf = $dp-&gt;get( $schain[$i] * 0x200, 0x200 );
$sfat = new DataProvider( $sfatbuf, DP_STRING_SOURCE );
//$this-&gt;dbglog-&gt;dump( strlen($sfatbuf), "strlen(\$sftabuf)");
//$this-&gt;dbglog-&gt;dump( $sfat, "\$sfat");
  if( $sfat-&gt;getSize() &lt; 0x200 ) {
/*DBG*/     $this-&gt;dbglog-&gt;debug("parse_workbook() function found (strlen($sfat) &lt; 0x200)  returns 6");
     return 6;
       }       
  for( $j=0; $jsfat[] = $sfat-&gt;getLong( $j * 4 );

   $sfat-&gt;close();
   unset( $sfatbuf, $sfat );
 } unset( $schain );
 $sfcbuf = $dp-&gt;ReadFromFat( $small_data_chain );
  $sdp = new DataProvider( $sfcbuf, DP_STRING_SOURCE );
  unset( $sfcbuf, $small_data_chain );
}$workbook_index = $this-&gt;find_stream( $dir, 'Workbook' );
if( $workbook_indexfind_stream( $dir, 'Book' );
if( $workbook_indexdbglog-&gt;debug("parse_workbook() function workbook index not found returns 7");
return 7;
}}$workbook_start_block = $dir-&gt;getLong( $workbook_index * 0x80 + 0x74 );
$workbook_length = $dir-&gt;getLong( $workbook_index * 0x80 + 0x78 );
$wb = '';
if( $workbook_length &gt; 0 ) {
if( $workbook_length &gt;= 0x1000 ) {
$chain = $this-&gt;get_blocks_chain($workbook_start_block);
$wb = $dp-&gt;ReadFromFat( $chain );
 } else {
$chain = $this-&gt;get_blocks_chain($workbook_start_block,true);
$wb = $sdp-&gt;ReadFromFat( $chain, 0x40 );
unset( $sdp );
 }$wb = substr($wb,0,$workbook_length);
if( strlen($wb) != $workbook_length )
return 6;
unset( $chain );
}// Unset fat arrays
unset( $this-&gt;fat, $this-&gt;sfat );
if( strlen($wb) dbglog-&gt;debug("parse_workbook() function workbook found (strlen($wb) dbglog-&gt;debug("parse_workbook() function workbook found (strlen($wb) &lt; 4) returns 6");
    return 6;
}// parse workbook header
if( strlen($wb) &lt; 256*ord($wb[3])+ord($wb[2]) ){
/*DBG*/ $this-&gt;dbglog-&gt;debug("parse_workbook() function workbook found (strlen($wb) &lt; 256*ord($wb[3])+ord($wb[2])) &lt; 4) returns 6");
return 6;
}if( ord($wb[0]) != 0x09 ){
/*DBG*/ $this-&gt;dbglog-&gt;debug("parse_workbook() function workbook found (ord($wb[0]) != 0x09) returns 6");
return 6;
}$vers = ord($wb[1]);
if( ($vers!=0) &amp;&amp; ($vers!=2) &amp;&amp; ($vers!=4) &amp;&amp; ($vers!=8) ){
return 8;
        }if( $vers!=8 )
 $biff_ver = ($ver+4)/2;
else {
if( strlen($wb) &lt; 12 ) return 6;
 switch( ord($wb[4])+256*ord($wb[5]) )
 {
case 0x0500:
if( ord($wb[0x0a])+256*ord($wb[0x0b]) &lt; 1994 )
$biff_ver = 5;
else {
switch(ord( $wb[8])+256*ord($wb[9]) ) {
 case 2412:
 case 3218:
 case 3321:
$biff_ver = 5;
break;
 default:
$biff_ver = 7;
break;
}}break;
case 0x0600:
$biff_ver = 8;
break;
default:
return 8;
 }}if( $biff_ver &lt; 5 ) return 8;
$ptr = 0;
$this-&gt;worksheet['offset'] = array();
$this-&gt;worksheet['options'] = array();
$this-&gt;worksheet['unicode'] = array();
$this-&gt;worksheet['name'] = array();
$this-&gt;worksheet['data'] = array();
$this-&gt;format = $this-&gt;populateFormat();
$this-&gt;fonts = array();
$this-&gt;fonts[0] = ExcelFont::basicFontRecord();
$this-&gt;xf = array();
$this-&gt;xf['format'] = array();
$this-&gt;xf['font'] = array();
$this-&gt;xf['type_prot'] = array();
$this-&gt;xf['alignment'] = array();
$this-&gt;xf['decoration'] = array();
$xf_cnt=0;
$this-&gt;sst['unicode'] = array();
$this-&gt;sst['data'] = array();
$opcode = 0;
$sst_defined = false;
$wblen = strlen($wb);
while( (ord($wb[$ptr])!=0x0a) &amp;&amp; ($ptrworksheet['offset'][] = $ofs;
$this-&gt;worksheet['options'][] = ord($wb[$ptr+8])+256*ord($wb[$ptr+9]);
if( $biff_ver==8 ) {
$len = ord($wb[$ptr+10]);
if( (ord($wb[$ptr+11]) &amp; 1) &gt; 0 ) {
 $this-&gt;worksheet['unicode'][] = true;
$len = $len*2;
 } else {
 $this-&gt;worksheet['unicode'][] = false;
 } $this-&gt;worksheet['name'][] = substr($wb,$ptr+12,$len);
} else {
$this-&gt;worksheet['unicode'][] = false;
$len = ord($wb[$ptr+10]);
$this-&gt;worksheet['name'][] = substr($wb,$ptr+11,$len);
}$pws = $this-&gt;parse_worksheet(substr($wb,$ofs));
if( is_array($pws) )
$this-&gt;worksheet['data'][] = $pws;
else
return $pws;
break;
 // Format
  case 0x041e:
   $fidx = ord($wb[$ptr+4])+256*ord($wb[$ptr+5]);
  if($fidx7)
    $this-&gt;format[$fidx] = $this-&gt;getUnicodeString($wb,$ptr+6);
        break;
 // FONT 0x31
   case EXCEL_FONT_RID:
                $rec = ExcelFont::getFontRecord($wb,$ptr+4);
                $this-&gt;fonts[count($this-&gt;fonts)] = $rec;
/*echo str_replace("\n","
\n",ExcelFont::toString($rec,count($this-&gt;fonts)-1));
echo "FontRecord
" */;
        break;
 // XF
  case 0x00e0:
  $this-&gt;xf['font'][$xf_cnt] = ord($wb[$ptr+4])+256*ord($wb[$ptr+5]);
  $this-&gt;xf['format'][$xf_cnt] = ord($wb[$ptr+6])+256*ord($wb[$ptr+7]);
  $this-&gt;xf['type'][$xf_cnt]  = "1";
  $this-&gt;xf['bitmask'][$xf_cnt] = "1";
  $xf_cnt++;
        break;
  // SST
  case 0x00fc:
if( $biff_ver &lt; 8 ) break;
$sbuflen = ord($wb[$ptr+2]) + 256*ord($wb[$ptr+3]);
if( $oc != 0x3c ) {
 if( $sst_defined ) return 6;
$snum = ExcelParserUtil::str2long(substr($wb,$ptr+8,4));
$sptr = $ptr+12;
$sst_defined = true;
} else {
 if( $rslen &gt; $slen ) {
$sptr = $ptr+4;
$rslen -= $slen;
$slen = $rslen;
if( (ord($wb[$sptr]) &amp; 1) &gt; 0 ) {
 if( $char_bytes == 1 ) {
  $sstr = '';
for( $i=0; $i $ptr+4+$sbuflen )
$slen = ($ptr+$sbuflen-$sptr+3)/$schar_bytes;
$sstr = substr($wb,$sptr+1,$slen*$schar_bytes);
if( ($char_bytes == 2) &amp;&amp; ($schar_bytes == 1) )
{
$sstr2 = '';
for( $i=0; $i= strlen($wb)) ||
    ($sptr &lt; $ptr+4+$sbuflen) ||
    (ord($wb[$sptr]) != 0x3c) )
{
    return 6;
}break;
 } else {
if( $char_bytes == 2 ) {
$this-&gt;sst['unicode'][] = true;
} else {
$this-&gt;sst['unicode'][] = false;
}$this-&gt;sst['data'][] = $str;
$snum--;
 } } else {
$sptr = $ptr+4;
if( $sptr &gt; $ptr ) $sptr += 4*$rt+$fesz;
 }}while(  ($sptr &lt; $ptr+4+$sbuflen) &amp;&amp;
($sptr &lt; strlen($wb)) &amp;&amp;
($snum &gt; 0) )
{
 $rslen = ord($wb[$sptr])+256*ord($wb[$sptr+1]);
 $slen = $rslen;
 if( (ord($wb[$sptr+2]) &amp; 1) &gt; 0 ) {
$char_bytes = 2;
 } else {
$char_bytes = 1;
 } $rt = 0;
 $fesz = 0;
 switch (ord($wb[$sptr+2]) &amp; 0x0c) {
  // Rich-Text with Far-East
  case 0x0c:
$rt = ord($wb[$sptr+3])+256*(ord($wb[$sptr+4]));
$fesz = ExcelParserUtil::str2long(substr($wb,$sptr+5,4));
if( $sptr+9+$slen*$char_bytes &gt; $ptr+4+$sbuflen )
$slen = ($ptr+$sbuflen-$sptr-5)/$char_bytes;
$str = substr($wb,$sptr+9,$slen*$char_bytes);
$sptr += $slen*$char_bytes+9;
break;
  // Rich-Text
  case 8:
$rt = ord($wb[$sptr+3])+256*(ord($wb[$sptr+4]));
if( $sptr+5+$slen*$char_bytes &gt; $ptr+4+$sbuflen )
$slen = ($ptr+$sbuflen-$sptr-1)/$char_bytes;
$str = substr($wb,$sptr+5,$slen*$char_bytes);
$sptr += $slen*$char_bytes+5;
break;
  // Far-East
  case 4:
$fesz = ExcelParserUtil::str2long(substr($wb,$sptr+3,4));
if( $sptr+7+$slen*$char_bytes &gt; $ptr+4+$sbuflen )
$slen = ($ptr+$sbuflen-$sptr-3)/$char_bytes;
$str = substr($wb,$sptr+7,$slen*$char_bytes);
$sptr += $slen*$char_bytes+7;
break;
  // Compressed or uncompressed unicode
  case 0:
if( $sptr+3+$slen*$char_bytes &gt; $ptr+4+$sbuflen )
$slen = ($ptr+$sbuflen-$sptr+1)/$char_bytes;
 $str = substr($wb,$sptr+3,$slen*$char_bytes);
 $sptr += $slen*$char_bytes+3;
break;
 } if( $slen &lt; $rslen ) {
if( ($sptr &gt;= strlen($wb)) ||
    ($sptr &lt; $ptr+4+$sbuflen) ||
    (ord($wb[$sptr]) != 0x3c) ) return 6;
 } else {
if( $char_bytes == 2 ) {
$this-&gt;sst['unicode'][] = true;
} else {
$this-&gt;sst['unicode'][] = false;
}$sptr += 4*$rt+$fesz;
$this-&gt;sst['data'][] = $str;
 $snum--;
 }} // switch
break;
 } // switch
// !!! Optimization:
//  $this-&gt;wsb[] = substr($wb,$ptr,4+256*ord($wb[$ptr+3])+ord($wb[$ptr+2]));
$ptr += 4+256*ord($wb[$ptr+3])+ord($wb[$ptr+2]);
} // while
// !!! Optimization:
//  $this-&gt;workbook = $wb;
$this-&gt;biff_version = $biff_ver;
/*DBG*/ $this-&gt;dbglog-&gt;debug("parse_workbook() function returns 0");
return 0;
}// ParseFromString &amp; ParseFromFile
//---------------------------------
//
// IN:
//string contents - File contents
//string filename - File name of an existing Excel file.
//
// OUT:
//0 - success
//1 - can't open file
//2 - file too small to be an Excel file
//3 - error reading header
//4 - error reading file
//5 - This is not an Excel file or file stored in &lt; Excel 5.0
//6 - file corrupted
//7 - data not found
//8 - Unsupported file version
function ParseFromString( $contents )
{
$this-&gt;dbglog-&gt;info("ParseFromString() enter.");
$this-&gt;dp = new DataProvider( $contents, DP_STRING_SOURCE );
return $this-&gt;InitParser();
}function ParseFromFile( $filename )
{
$this-&gt;dbglog-&gt;info("ParseFromFile() enter.");
$this-&gt;dp = new DataProvider( $filename, DP_FILE_SOURCE );
return $this-&gt;InitParser();
}function InitParser()
{
$this-&gt;dbglog-&gt;info("InitParser() enter.");
if( !$this-&gt;dp-&gt;isValid() )
{
$this-&gt;dbglog-&gt;error("InitParser() Failed to open file.");
$this-&gt;dbglog-&gt;error("InitParser() function returns 1");
return 1;
}if( $this-&gt;dp-&gt;getSize() dbglog-&gt;error("InitParser() File too small to be an Excel file.");
$this-&gt;dbglog-&gt;error("InitParser() function returns 2");
return 2;
}$this-&gt;max_blocks = $this-&gt;dp-&gt;getBlocks();
// read file header
$hdrbuf = $this-&gt;dp-&gt;get( 0, 0x200 );
if( strlen( $hdrbuf ) &lt; 0x200 )
{
$this-&gt;dbglog-&gt;error("InitParser() Error reading header.");
$this-&gt;dbglog-&gt;error("InitParser() function returns 3");
return 3;
}// check file header
$header_sig = array(0xd0,0xcf,0x11,0xe0,0xa1,0xb1,0x1a,0xe1);
for( $i = 0; $i &lt; count($header_sig); $i++ )
if( $header_sig[$i] != ord( $hdrbuf[$i] ) ){
/*DBG*/        $this-&gt;dbglog-&gt;error("InitParser() function founds invalid header");
/*DBG*/        $this-&gt;dbglog-&gt;error("InitParser() function returns 5");
return 5;
            }$f_header = new DataProvider( $hdrbuf, DP_STRING_SOURCE );
unset( $hdrbuf, $header_sig, $i );
$this-&gt;dp-&gt;_baseOfs = 0x200;
$rc = $this-&gt;parse_workbook( $f_header, $this-&gt;dp );
unset( $f_header );
unset( $this-&gt;dp, $this-&gt;max_blocks, $this-&gt;max_sblocks );
return $rc;
}}function uc2html($str) {
return $str;
}

//------------------------¶ÁÈ¡ExcelÎÄ¼þ
function Read_Excel_File($ExcelFile,&amp;$result) {

$exc = new ExcelFileParser("", ABC_NO_LOG );
$res=$exc-&gt;ParseFromFile($ExcelFile);$result=null;

switch ($res) {
	case 0: break;
	case 1: $err="ÎÞ·¨´ò¿ªÎÄ¼þ"; break;
	case 2: $err="ÎÄ¼þÌ«Ð¡£¬¿ÉÄÜ²»ÊÇExcelÎÄ¼þ"; break;
	case 3: $err="ÎÄ¼þÍ·¶ÁÈ¡´íÎó"; break;
	case 4: $err="¶ÁÈ¡ÎÄ¼þÊ±³ö´í"; break;
	case 5: $err="Õâ²»ÊÇÒ»¸öExcelÎÄ¼þ»òÕßÊÇExcel5.0ÒÔÇ°°æ±¾ÎÄ¼þ"; break;
	case 6: $err="ÎÄ¼þËð»µ"; break;
	case 7: $err="ÔÚÎÄ¼þÖÐÃ»ÓÐ·¢ÏÖExcelÊý¾Ý"; break;
	case 8: $err="²»Ö§³ÖµÄÎÄ¼þ°æ±¾"; break;
	default:
		$err="Î´Öª´íÎó"; break;
}

for( $ws_num=0; $ws_numworksheet['name']); $ws_num++ )
{
	$Sheetname=$exc-&gt;worksheet['name'][$ws_num];	
	$ws = $exc-&gt;worksheet['data'][$ws_num];	
	for( $j=0; $jsst['unicode'][$ind] ) {
					$s = uc2html($exc-&gt;sst['data'][$ind]);
				} else
					$s = $exc-&gt;sst['data'][$ind];
				if( strlen(trim($s))==0 )
					$V="";
				else
					$V=$s;
				break;
			// integer number
			case 1:
				$V=(int)($data['data']);
				break;
			// float number
			case 2:
				$V=(float)($data['data']);
				break;
			// date
			case 3:
				$ret = $exc-&gt;getDateArray($data['data']);
				$V=$ret['year']."-".$ret['month']."-".$ret['day']." ".$ret['hour'];
				break;
			default:
				break;
		   }											
			$result[$Sheetname][$j][$i]=$V;
		}		
	}
}
if ($err=='') {return 0;} else {return $err;}
}

//------------------------½¨Á¢ExcelÎÄ¼þ
function Create_Excel_File($ExcelFile,$Data) {

header ('Content-type: application/x-msexcel'); 
header ("Content-Disposition: attachment; filename=$ExcelFile" );  

function xlsBOF() { 
    echo pack("ssssss", 0x809, 0x8, 0x0, 0x10, 0x0, 0x0);  
    return; 
}
function xlsEOF() { 
    echo pack("ss", 0x0A, 0x00); 
    return; 
} 
function xlsWriteNumber($Row, $Col, $Value) { 
    echo pack("sssss", 0x203, 14, $Row, $Col, 0x0); 
    echo pack("d", $Value); 
    return; 
} 
function xlsWriteLabel($Row, $Col, $Value ) { 
    $L = strlen($Value); 
    echo pack("ssssss", 0x204, 8 + $L, $Row, $Col, 0x0, $L); 
    echo $Value; 
return; 
} 

xlsBOF();
for ($i=0;$i</pre>