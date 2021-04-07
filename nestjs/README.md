### @Inject(custom_name)

```
if u don't want to import by class and u want to custom provider's name
---
@Module({
  providers: [
    {
      provide: 'custom_name_A_service',
      useClass: AService,
    },
  ],
  exports: ['custom_name_A_service'],
})
export class AModule {}
----
@Module({
  imports: [AModule]
  providers:[Bservice]
  exports: ['custom_name_A_service'],
})
export class BModule {}
---
class BService {
    @Inject('custom_name_A_service') private readonly aService: AService,
}

https://docs.nestjs.com/fundamentals/custom-providers
```
