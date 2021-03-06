#rule engine lib build
ADD_LIBRARY(ruleengine SHARED
	pln/BackwardChainer.cc
	pln/PLNCommons.cc
	pln/ForwardChainer.cc
	pln/BCPatternMatch.cc
	pln/ForwardChainInputMatchCB.cc
	pln/ForwardChainPatternMatchCB.cc
	pln/ForwardChainerCallBack.h
	pln/DefaultForwardChainerCB.cc
	pln/FCMemory.cc	
	RuleEngineModule.cc
	InferenceSCM.cc
	Rule.cc
	JsonicControlPolicyParamLoader.cc
)

ADD_DEPENDENCIES(ruleengine
	opencog_atom_types
)

TARGET_LINK_LIBRARIES(ruleengine
	lambda
	query
	smob
	execution
	atomutils
	atomspace
)

# Build the RuelEngine cogserver module
ADD_LIBRARY(RuleEngineModule SHARED
	RuleEngineModule.cc
)

TARGET_LINK_LIBRARIES(RuleEngineModule ruleengine server)


# PLNModule
IF (TBB_FOUND)
	INCLUDE_DIRECTORIES (
		${TBB_INCLUDE_DIR}
	)

	LINK_DIRECTORIES(
		${TBB_INCLUDE_DIR}
		${TBB_LIBRARY_DIRS}
	)

	ADD_LIBRARY(plnmodule SHARED
		pln/PLNModule.cc
	)

	TARGET_LINK_LIBRARIES(plnmodule
		server
		tbb
		ruleengine
	)
ENDIF (TBB_FOUND)


IF (WIN32)
   INSTALL (TARGETS query DESTINATION "lib${LIB_DIR_SUFFIX}/opencog")
ELSEIF (CYGWIN)
   INSTALL (TARGETS query DESTINATION "lib${LIB_DIR_SUFFIX}/opencog")
ENDIF (WIN32)

INSTALL (FILES
	pln/ForwardChainer.h
	pln/Chainer.h
	pln/BCPatternMatch.h
	pln/ForwardChainInputMatchCB.h
	pln/ForwardChainPatternMatchCB.h
	DESTINATION "include/${PROJECT_NAME}/reasoning/RuleEngine/rule-engine-src"
)

