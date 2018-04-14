# demo_download_file

##	实现文件下载

   下面是一个简单的实现文件下载功能的代码：

```kotlin
package com.easy.springboot.demo_download_file

import org.springframework.core.io.FileSystemResource
import org.springframework.core.io.InputStreamResource
import org.springframework.http.HttpHeaders
import org.springframework.http.MediaType
import org.springframework.http.ResponseEntity
import org.springframework.stereotype.Controller
import org.springframework.web.bind.annotation.RequestMapping
import org.springframework.web.bind.annotation.RequestMethod

@Controller
class DownloadController {
    @RequestMapping(value = ["/download"], method = [RequestMethod.GET])
    fun downloadFile(log: String): ResponseEntity<InputStreamResource> {
        val filePath = "/Users/jack/logs/" + log
        val file = FileSystemResource(filePath)
        val headers = HttpHeaders()
        headers.add("Cache-Control", "no-cache, no-store, must-revalidate");
        headers.add("Content-Disposition", String.format("attachment; filename=\"%s\"", file.getFilename()))
        headers.add("Pragma", "no-cache")
        headers.add("Expires", "0")

        return ResponseEntity
                .ok()
                .headers(headers)
                .contentLength(file.contentLength())
                .contentType(MediaType.parseMediaType("application/octet-stream"))
                .body(InputStreamResource(file.getInputStream()))
    }

}
```

其中，contentType()方法中设置文件类型为application/octet-stream，从而以流的形式下载文件，这样可以实现任意格式的文件下载。
浏览器访问http://127.0.0.1:8080/download?log=demo_logging，可以实现本机 /Users/jack/logs/demo_logging 文件的下载。
