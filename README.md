# nest-microservices-messaging
- A NestJS module for `generator-nest-microservices` yeoman template for distributed messaging. This package uses https://www.npmjs.com/package/@golevelup/nestjs-rabbitmq
### How to use
- install package with `npm i -S nest-microservices-messaging`
- Register the `MessagingModule` to your feature modules:
```
@Module({})
export class AppModule {
  static register(config: ConfigOptions): DynamicModule {
    return {
      module: AppModule,
      imports: [
        MessagingModule.register(config.messagingOptions),
        ...
      ],
    };
  }
}
```
- Once `MessagingModule` is registered, then you can use `PublisherService` to publish to RabbitMQ
```
export class ExampleController {
  constructor (private service: PublisherService) {
    super (service, logger);
  }

  await publish () {
    this.publisherService.publish('my-queue', { hello: 'world' });
  }
}
```
### Messaging options
|Property|Type|Required|Description|
|-|-|-|-|
|exchanges|`MessageQueueOptions`|true| Contains { name: string, type: string } which defined the exchange name and type of exchange
|uri|`String`|true| Uri or your RabbitMQ server
|connectionInitOptions|`MessageConnectionOptions`|true| Contains { wait: boolean } for connection resiliency. See https://www.npmjs.com/package/@golevelup/nestjs-rabbitmq for more details
