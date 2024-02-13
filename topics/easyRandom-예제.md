# easyRandom 예제

    public User createUser(int seed) {
        Predicate<Field> id = named("id")
                .and(ofType(Long.class))
                .and(inClass(User.class));

        Predicate<Field> createdAt = named("createdAt")
                .and(ofType(LocalDateTime.class))
                .and(inClass(BaseTimeEntity.class));

        Predicate<Field> updatedAt = named("updatedAt")
                .and(ofType(LocalDateTime.class))
                .and(inClass(BaseTimeEntity.class));

        Predicate<Field> age = named("age")
                .and(ofType(Integer.class))
                .and(inClass(User.class));

        Predicate<Field> name = named("name")
                .and(ofType(String.class))
                .and(inClass(User.class));

        Predicate<Field> role = named("role")
                .and(ofType(Role.class))
                .and(inClass(User.class));

        Predicate<Field> phone = named("phone")
                .and(ofType(String.class))
                .and(inClass(User.class));

        EasyRandomParameters parameters = new EasyRandomParameters()
                .seed(seed)
                .excludeField(id)
                .excludeField(createdAt)
                .excludeField(updatedAt)
                .randomize(age, () -> new Random().nextInt(20, 80))
                .randomize(name, () -> new NameRandomizer().getRandomValue())
                .randomize(role, () -> Role.CUSTOMER)
                .randomize(phone, () -> new PhoneRandomizer().getRandomValue());

        return new EasyRandom(parameters).nextObject(User.class);
    }