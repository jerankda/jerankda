Future<void> updateObjects<T>({required Type type}) async {
    final Map<Type, Map> typeToMap = {
      Annotation: annotations,
      Location: locations,
      Driver: drivers,
      QuestionCategory: categories,
      Question: questions,
      TestDrive: testDrives,
      Project: projects,
      LightSystem: lightsystems,
    };

    if (!typeToMap.containsKey(type)) {
      log.severe('Error while updating Objects of Type $type: Type not found');
      throw Exception('Type not found');
    } else {
      log.info('Updating Objects of Type $type');
      try {
        List objectList =
            await _database.httpResolver.getAllObjectsOfType(objectType: type);
        for (var object in objectList) {
          if (type == Project) {
            typeToMap[type]![object?.name] = object;
          } else {
            typeToMap[type]![object?.id] = object;
          }
        }
      } catch (e) {
        log.severe('Error while updating Objects of Type $type: $e');
        rethrow;
      }
    }
  }
