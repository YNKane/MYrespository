package com.example.demo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.web.bind.annotation.*;

@RestController
@SpringBootApplication
public class DemoApplication {
	
	@Autowired
	private Hello helloworld;
	@RequestMapping("/")
	 String home() {
		return helloworld.print();
	 }
	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}
}


package com.example.demo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Profile;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

import com.fasterxml.jackson.core.sym.Name;

@Component
@Profile({"hello","dev"})
public class SayHello implements Hello {

	@Value("${name:world}")
	private String name;
	@Value("${hello:hello}")
	private String hello;
	@Override
	public String print() {
		// TODO Auto-generated method stub
		return hello+" "+name;
	}
}

name: Alice
hello: Good morning
spring:
  profiles: dev
---
name: Jane
hello: GoodBye
spring:
  profiles: bye
---
spring:
  profiles: 
    active: bye


  
