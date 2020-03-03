```
package com.enliple.recommend.monitor.study;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

import javax.annotation.Resource;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.stereotype.Component;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest(classes= {
	DIStudy.class, 
	DIStudy.ACreatable.class, 
	DIStudy.BCreatable.class, 
	DIStudy.Creatable.class
})
public class DIStudy {
	
	@Resource(name = "aCreatable") private Creatable aCreatable;
	@Resource(name = "bCreatable") private Creatable bCreatable;
	
	public interface Creatable {
		String create();
	}
	
	@Component("aCreatable")
	class ACreatable implements Creatable {
		@Override
		public String create() {
			return this.getClass().getSimpleName();
		}
	}
	
	@Component("bCreatable")
	class BCreatable implements Creatable {
		@Override
		public String create() {
			return this.getClass().getSimpleName();
		}
	}
	
	@Test
	public void t0() {
		assertThat(aCreatable.create(), is(ACreatable.class.getSimpleName()));
		assertThat(bCreatable.create(), is(BCreatable.class.getSimpleName()));
	}
}

```
