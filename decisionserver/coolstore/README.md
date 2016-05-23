Example BRMS Rule Service for CoolStore
=======================================

Tested on BRMS 6.2

Sample request payload `request.xml`:

	<batch-execution lookup="CoolSession">
	  <insert out-identifier="sc-identifier" return-object="true" entry-point="DEFAULT">
	    <com.redhat.coolstore.ShoppingCart>
	      <cartItemPromoSavings>0.0</cartItemPromoSavings>
	      <cartItemTotal>0.0</cartItemTotal>
	      <cartTotal>0.0</cartTotal>
	      <shippingPromoSavings>0.0</shippingPromoSavings>
	      <shippingTotal>0.0</shippingTotal>
	    </com.redhat.coolstore.ShoppingCart>
	  </insert>
	  <insert out-identifier="sci-identifier" return-object="true" entry-point="DEFAULT">
	    <com.redhat.coolstore.ShoppingCartItem>
	      <itemId>123</itemId>
	      <name>Test</name>
	      <price>10.0</price>
	      <promoSavings>0.0</promoSavings>
	      <quantity>1</quantity>
	      <shoppingCart reference="../../../insert/com.redhat.coolstore.ShoppingCart"/>
	    </com.redhat.coolstore.ShoppingCartItem>
	  </insert>
	  <insert out-identifier="ps-identifier" return-object="false" entry-point="Promo Stream">
	    <com.redhat.coolstore.PromoEvent>
	      <itemId>123</itemId>
	      <percentOff>0.50</percentOff>
	    </com.redhat.coolstore.PromoEvent>
	  </insert>
	  <start-process processId="com.redhat.coolstore.PriceProcess"/>
	  <fire-all-rules out-identifier="fire-identifier"/>
	</batch-execution>

Samle `curl` invocation (username: kieserver password: kieserver1!)

	curl -X POST \
	  -d @request.xml \
	  -H "Accept:application/json" \
	  -H "X-KIE-ContentType:XSTREAM" \
	  -H "Content-Type:application/xml" \
	  -H "Authorization:Basic a2llc2VydmVyOmtpZXNlcnZlcjEh" \
	  -H "X-KIE-ClassType:org.drools.core.command.runtime.BatchExecutionCommandImpl" \
	http://host/kie-server/services/rest/server/containers/instances/CoolContainer